---
title: "a lemez tooa Linux virtuális gép az Azure-ban aaaAttach |} Microsoft Docs"
description: "Ismerje meg, hogyan tooattach az adatok lemezre tooa Linux virtuális gép hello klasszikus telepítési modell és hello lemez inicializálása, hogy készen álljon a használatra"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4901384d-2a6f-4f46-bba0-337a348b7f87
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: c76d8479ac2b522d2b6df658cd28f242473f30ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-data-disk-tooa-linux-virtual-machine"></a><span data-ttu-id="843e2-103">Hogyan tooAttach egy adatlemez tooa Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="843e2-103">How tooAttach a Data Disk tooa Linux Virtual Machine</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="843e2-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="843e2-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="843e2-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="843e2-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="843e2-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="843e2-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="843e2-107">Lásd: hogyan túl[hello erőforrás-kezelő telepítési modellel adatlemezzel](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="843e2-107">See how too[attach a data disk using hello Resource Manager deployment model](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="843e2-108">Csatolhat is üres és a lemezek, amelyek tartalmazzák az adatok tooyour Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="843e2-108">You can attach both empty disks and disks that contain data tooyour Azure VMs.</span></span> <span data-ttu-id="843e2-109">Mindkét típusú lemezek a .vhd fájlok találhatók az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="843e2-109">Both types of disks are .vhd files that reside in an Azure storage account.</span></span> <span data-ttu-id="843e2-110">Adja hozzá a lemezt tooa Linux gépi, hello lemez csatlakoztatása után meg kell tooinitialize és formázza azt, hogy készen álljon a használatra.</span><span class="sxs-lookup"><span data-stu-id="843e2-110">As with adding any disk tooa Linux machine, after you attach hello disk you need tooinitialize and format it so it's ready for use.</span></span> <span data-ttu-id="843e2-111">Ez a cikk adatokat is üres és a lemezek már tartalmazó adatok tooyour virtuális gépeket, valamint, hogy toothen inicializálása és formázása új lemez csatolása.</span><span class="sxs-lookup"><span data-stu-id="843e2-111">This article details attaching both empty disks and disks already containing data tooyour VMs, as well as how toothen initialize and format a new disk.</span></span>

> [!NOTE]
> <span data-ttu-id="843e2-112">A bevált gyakorlat toouse egy vagy több különálló lemezek toostore egy virtuális gép adatait.</span><span class="sxs-lookup"><span data-stu-id="843e2-112">It's a best practice toouse one or more separate disks toostore a virtual machine's data.</span></span> <span data-ttu-id="843e2-113">Egy Azure virtuális gép létrehozásakor rendelkezik az operációs rendszer és egy ideiglenes lemezzel.</span><span class="sxs-lookup"><span data-stu-id="843e2-113">When you create an Azure virtual machine, it has an operating system disk and a temporary disk.</span></span> <span data-ttu-id="843e2-114">**Ne használjon hello ideiglenes toostore állandó lemezadatokat.**</span><span class="sxs-lookup"><span data-stu-id="843e2-114">**Do not use hello temporary disk toostore persistent data.**</span></span> <span data-ttu-id="843e2-115">Hello név azt jelenti, mert csak az átmeneti tárolási biztosít.</span><span class="sxs-lookup"><span data-stu-id="843e2-115">As hello name implies, it provides temporary storage only.</span></span> <span data-ttu-id="843e2-116">Kínál nincs redundancia vagy a biztonsági mentést, mert az nem található az Azure storage.</span><span class="sxs-lookup"><span data-stu-id="843e2-116">It offers no redundancy or backup because it doesn't reside in Azure storage.</span></span>
> <span data-ttu-id="843e2-117">hello ideiglenes lemez általában hello Azure Linux ügynök által felügyelt, és automatikusan csatlakoztatott túl**/mnt és az erőforrások** (vagy **/mnt** Ubuntu képek).</span><span class="sxs-lookup"><span data-stu-id="843e2-117">hello temporary disk is typically managed by hello Azure Linux Agent and automatically mounted too**/mnt/resource** (or **/mnt** on Ubuntu images).</span></span> <span data-ttu-id="843e2-118">A hello ugyanakkor, adatlemezt neve lehet hello Linux kernel hasonlót `/dev/sdc`, és toopartition, kell formázni, és a csatlakoztatási ehhez az erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="843e2-118">On hello other hand, a data disk might be named by hello Linux kernel something like `/dev/sdc`, and you need toopartition, format, and mount this resource.</span></span> <span data-ttu-id="843e2-119">Lásd: hello [Azure Linux ügynök felhasználói útmutató] [ Agent] részleteiről.</span><span class="sxs-lookup"><span data-stu-id="843e2-119">See hello [Azure Linux Agent User Guide][Agent] for details.</span></span>
> 
> 

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a><span data-ttu-id="843e2-120">A Linux új adatlemezt inicializálása</span><span class="sxs-lookup"><span data-stu-id="843e2-120">Initialize a new data disk in Linux</span></span>
1. <span data-ttu-id="843e2-121">SSH tooyour virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="843e2-121">SSH tooyour VM.</span></span> <span data-ttu-id="843e2-122">További információkért lásd: [hogyan toolog a Linux rendszerű virtuális gép tooa][Logon].</span><span class="sxs-lookup"><span data-stu-id="843e2-122">For more information, see [How toolog on tooa virtual machine running Linux][Logon].</span></span>
2. <span data-ttu-id="843e2-123">Toofind hello eszközazonosító ezután a hello adatok lemez tooinitialize kell.</span><span class="sxs-lookup"><span data-stu-id="843e2-123">Next you need toofind hello device identifier for hello data disk tooinitialize.</span></span> <span data-ttu-id="843e2-124">Két módon toodo van, amely:</span><span class="sxs-lookup"><span data-stu-id="843e2-124">There are two ways toodo that:</span></span>
   
    <span data-ttu-id="843e2-125">a) Grep a SCSI-eszközökhöz a hello jelentkezik, például a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="843e2-125">a) Grep for SCSI devices in hello logs, such as in hello following command:</span></span>
   
    ```bash
    sudo grep SCSI /var/log/messages
    ```
   
    <span data-ttu-id="843e2-126">A legutóbbi Ubuntu azokat a terjesztéseket, szükség lehet a toouse `sudo grep SCSI /var/log/syslog` , mert túl naplózás`/var/log/messages` alapértelmezés szerint le lehetnek tiltva.</span><span class="sxs-lookup"><span data-stu-id="843e2-126">For recent Ubuntu distributions, you may need toouse `sudo grep SCSI /var/log/syslog` because logging too`/var/log/messages` might be disabled by default.</span></span>
   
    <span data-ttu-id="843e2-127">Hello utolsó adatlemez, amely hozzá lett adva a megjelenített köszönőüzenetei hello azonosítója található.</span><span class="sxs-lookup"><span data-stu-id="843e2-127">You can find hello identifier of hello last data disk that was added in hello messages that are displayed.</span></span>
   
    ![Lemez köszönőüzenetei beolvasása](./media/attach-disk/scsidisklog.png)
   
    <span data-ttu-id="843e2-129">VAGY</span><span class="sxs-lookup"><span data-stu-id="843e2-129">OR</span></span>
   
    <span data-ttu-id="843e2-130">b) használata hello `lsscsi` parancs toofind hello eszközazonosító ki. `lsscsi` telepítheti vagy `yum install lsscsi` (Red Hat a disztribúció alapján) vagy `apt-get install lsscsi` (a Debian disztribúció alapján).</span><span class="sxs-lookup"><span data-stu-id="843e2-130">b) Use hello `lsscsi` command toofind out hello device id. `lsscsi` can be installed by either `yum install lsscsi` (on Red Hat based distributions) or `apt-get install lsscsi` (on Debian based distributions).</span></span> <span data-ttu-id="843e2-131">Hello lemez által keresett megtalálhatja a *lun* vagy **logikaiegység-szám**.</span><span class="sxs-lookup"><span data-stu-id="843e2-131">You can find hello disk you are looking for by its *lun* or **logical unit number**.</span></span> <span data-ttu-id="843e2-132">Például hello *lun* a csatolt hello lemezeket is könnyen látható `azure vm disk list <virtual-machine>` mint:</span><span class="sxs-lookup"><span data-stu-id="843e2-132">For example, hello *lun* for hello disks you attached can be easily seen from `azure vm disk list <virtual-machine>` as:</span></span>

    ```azurecli
    azure vm disk list myVM
    ```

    <span data-ttu-id="843e2-133">hello hasonló toohello következő kimenete:</span><span class="sxs-lookup"><span data-stu-id="843e2-133">hello output is similar toohello following:</span></span>

    ```azurecli
    info:    Executing command vm disk list
    + Fetching disk images
    + Getting virtual machines
    + Getting VM disks
    data:    Lun  Size(GB)  Blob-Name                         OS
    data:    ---  --------  --------------------------------  -----
    data:         30        myVM-2645b8030676c8f8.vhd  Linux
    data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
    info:    vm disk list command OK
    ```
   
    <span data-ttu-id="843e2-134">Hasonlítsa össze ezeket az adatokat a hello kimenete `lsscsi` hello az azonos virtuális gép mintában:</span><span class="sxs-lookup"><span data-stu-id="843e2-134">Compare this data with hello output of `lsscsi` for hello same sample virtual machine:</span></span>
   
    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```
   
    <span data-ttu-id="843e2-135">hello utolsó hello rekordot sorokban értéke hello *lun*.</span><span class="sxs-lookup"><span data-stu-id="843e2-135">hello last number in hello tuple in each row is hello *lun*.</span></span> <span data-ttu-id="843e2-136">Lásd: `man lsscsi` további információt.</span><span class="sxs-lookup"><span data-stu-id="843e2-136">See `man lsscsi` for more information.</span></span>
3. <span data-ttu-id="843e2-137">Hello parancssorba írja be a következő parancs toocreate hello eszközét:</span><span class="sxs-lookup"><span data-stu-id="843e2-137">At hello prompt, type hello following command toocreate your device:</span></span>
   
    ```bash
    sudo fdisk /dev/sdc
    ```

4. <span data-ttu-id="843e2-138">Amikor a rendszer kéri, írja be a  **n**  toocreate partíció.</span><span class="sxs-lookup"><span data-stu-id="843e2-138">When prompted, type **n** toocreate a partition.</span></span>

    ![Eszköz létrehozása](./media/attach-disk/fdisknewpartition.png)

5. <span data-ttu-id="843e2-140">Amikor a rendszer kéri, írja be a **p** toomake hello partíció hello elsődleges partíció.</span><span class="sxs-lookup"><span data-stu-id="843e2-140">When prompted, type **p** toomake hello partition hello primary partition.</span></span> <span data-ttu-id="843e2-141">Típus **1** toomake hello első partíció, és írja be hello palack tooaccept hello alapértelmezett értéket adja meg.</span><span class="sxs-lookup"><span data-stu-id="843e2-141">Type **1** toomake it hello first partition, and then type enter tooaccept hello default value for hello cylinder.</span></span> <span data-ttu-id="843e2-142">Egyes rendszerek képes hello alapértelmezett értékeinek hello megjelenítése első és utolsó szektorok, hello palack hello.</span><span class="sxs-lookup"><span data-stu-id="843e2-142">On some systems, it can show hello default values of hello first and hello last sectors, instead of hello cylinder.</span></span> <span data-ttu-id="843e2-143">Választhat tooaccept ezeket az alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="843e2-143">You can choose tooaccept these defaults.</span></span>

    ![A partíció](./media/attach-disk/fdisknewpartdetails.png)


6. <span data-ttu-id="843e2-145">Típus **p** hello lemez, amely alatt a toosee hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="843e2-145">Type **p** toosee hello details about hello disk that is being partitioned.</span></span>

    ![Lista fizikai lemezeinek adatai](./media/attach-disk/fdiskpartitiondetails.png)


7. <span data-ttu-id="843e2-147">Típus **w** hello lemezek toowrite hello beállításai.</span><span class="sxs-lookup"><span data-stu-id="843e2-147">Type **w** toowrite hello settings for hello disk.</span></span>

    ![Hello lemezen változások írása](./media/attach-disk/fdiskwritedisk.png)

8. <span data-ttu-id="843e2-149">Mostantól létrehozhat hello fájlrendszer hello új partíciót.</span><span class="sxs-lookup"><span data-stu-id="843e2-149">Now you can create hello file system on hello new partition.</span></span> <span data-ttu-id="843e2-150">Hello számú toohello eszköz Partícióazonosító hozzáfűzése (a példában a következő hello `/dev/sdc1`).</span><span class="sxs-lookup"><span data-stu-id="843e2-150">Append hello partition number toohello device ID (in hello following example `/dev/sdc1`).</span></span> <span data-ttu-id="843e2-151">hello alábbi példa partíciót hoz létre ext4 /dev/sdc1 meg:</span><span class="sxs-lookup"><span data-stu-id="843e2-151">hello following example creates an ext4 partition on /dev/sdc1:</span></span>
   
    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
   
    ![-Fájl létrehozása](./media/attach-disk/mkfsext4.png)
   
   > [!NOTE]
   > <span data-ttu-id="843e2-153">SuSE Linux Enterprise 11 rendszerek csak ext4 fájlrendszerekhez támogatja a csak olvasási hozzáféréssel.</span><span class="sxs-lookup"><span data-stu-id="843e2-153">SuSE Linux Enterprise 11 systems only support read-only access for ext4 file systems.</span></span> <span data-ttu-id="843e2-154">Ezek a rendszerek tooformat hello új fájlrendszer ext4, hanem ext3 ajánlott.</span><span class="sxs-lookup"><span data-stu-id="843e2-154">For these systems, it is recommended tooformat hello new file system as ext3 rather than ext4.</span></span>

9. <span data-ttu-id="843e2-155">Győződjön meg a könyvtár toomount hello új fájlrendszer, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="843e2-155">Make a directory toomount hello new file system, as follows:</span></span>
   
    ```bash
    sudo mkdir /datadrive
    ```

10. <span data-ttu-id="843e2-156">Végül lehet hello meghajtót csatlakoztatni, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="843e2-156">Finally you can mount hello drive, as follows:</span></span>
   
    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```
   
    <span data-ttu-id="843e2-157">hello adatlemeze most már készen áll a toouse, **/datadrive**.</span><span class="sxs-lookup"><span data-stu-id="843e2-157">hello data disk is now ready toouse as **/datadrive**.</span></span>
   
    ![Hello directory és a csatlakoztatási hello lemez létrehozása](./media/attach-disk/mkdirandmount.png)

11. <span data-ttu-id="843e2-159">Hello új meghajtó túl/etc/fstab hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="843e2-159">Add hello new drive too/etc/fstab:</span></span>
   
    <span data-ttu-id="843e2-160">tooensure hello meghajtót csatlakoztatni automatikusan, kell legyen újraindítás toohello /etc/fstab fájl hozzáadása után.</span><span class="sxs-lookup"><span data-stu-id="843e2-160">tooensure hello drive is remounted automatically after a reboot it must be added toohello /etc/fstab file.</span></span> <span data-ttu-id="843e2-161">Emellett javasoljuk, hogy hello UUID (univerzálisan egyedi azonosítója) /etc/fstab toorefer toohello helyett csak hello eszköz nevét (pl. /dev/sdc1) szerepel.</span><span class="sxs-lookup"><span data-stu-id="843e2-161">In addition, it is highly recommended that hello UUID (Universally Unique IDentifier) is used in /etc/fstab toorefer toohello drive rather than just hello device name (i.e. /dev/sdc1).</span></span> <span data-ttu-id="843e2-162">Hello segítségével UUID elkerülhető hello helytelen lemezkonfigurációt megadott helyre, ha az operációs rendszer hello lemezhiba észleli a rendszerindítás során, és minden fennmaradó adatlemezt, majd alatt hozzárendelt azokat eszközazonosítókat csatlakoztatott tooa alatt.</span><span class="sxs-lookup"><span data-stu-id="843e2-162">Using hello UUID avoids hello incorrect disk being mounted tooa given location if hello OS detects a disk error during boot and any remaining data disks then being assigned those device IDs.</span></span> <span data-ttu-id="843e2-163">toofind hello UUID hello új meghajtó, használhatja a hello **blkid** segédprogram:</span><span class="sxs-lookup"><span data-stu-id="843e2-163">toofind hello UUID of hello new drive, you can use hello **blkid** utility:</span></span>
   
    ```bash
    sudo -i blkid
    ```
   
    <span data-ttu-id="843e2-164">hello kimenete a következő példa hasonló toohello néz ki:</span><span class="sxs-lookup"><span data-stu-id="843e2-164">hello output looks similar toohello following example:</span></span>
   
    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

    > [!NOTE]
    > <span data-ttu-id="843e2-165">Nem megfelelően szerkesztése hello **/etc/fstab** fájl meghiúsulását eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="843e2-165">Improperly editing hello **/etc/fstab** file could result in an unbootable system.</span></span> <span data-ttu-id="843e2-166">Ha nem ismeri, tekintse meg a toohello terjesztési dokumentációját olvashat, hogyan tooproperly szerkesztheti a fájlt.</span><span class="sxs-lookup"><span data-stu-id="843e2-166">If unsure, refer toohello distribution's documentation for information on how tooproperly edit this file.</span></span> <span data-ttu-id="843e2-167">Ajánlott továbbá, hogy létrejött-e a fájl biztonsági másolata hello /etc/fstab szerkesztése előtt.</span><span class="sxs-lookup"><span data-stu-id="843e2-167">It is also recommended that a backup of hello /etc/fstab file is created before editing.</span></span>

    <span data-ttu-id="843e2-168">Ezután nyissa meg a hello **/etc/fstab** fájlt egy szövegszerkesztőben:</span><span class="sxs-lookup"><span data-stu-id="843e2-168">Next, open hello **/etc/fstab** file in a text editor:</span></span>

    ```bash
    sudo vi /etc/fstab
    ```

    <span data-ttu-id="843e2-169">A jelen példában használjuk hello UUID hello az új **/dev/sdc1** hello előző lépést, és hello csatlakozásipont létrehozott eszköz **/datadrive**.</span><span class="sxs-lookup"><span data-stu-id="843e2-169">In this example, we use hello UUID value for hello new **/dev/sdc1** device that was created in hello previous steps, and hello mountpoint **/datadrive**.</span></span> <span data-ttu-id="843e2-170">Adja hozzá a következő sor toohello vége hello hello **/etc/fstab** fájlt:</span><span class="sxs-lookup"><span data-stu-id="843e2-170">Add hello following line toohello end of hello **/etc/fstab** file:</span></span>

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
    ```

    <span data-ttu-id="843e2-171">Vagy SuSE Linux-alapú rendszereken szükség lehet a toouse némileg eltérő formátuma:</span><span class="sxs-lookup"><span data-stu-id="843e2-171">Or, on systems based on SuSE Linux you may need toouse a slightly different format:</span></span>

    ```sh
    /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    ```

    > [!NOTE]
    > <span data-ttu-id="843e2-172">Hello `nofail` lehetőség biztosítja, hogy virtuális gép elindul, még akkor is, ha hello fájlrendszer sérült, vagy hello lemez nem létezik a rendszerindítás hello.</span><span class="sxs-lookup"><span data-stu-id="843e2-172">hello `nofail` option ensures that hello VM starts even if hello filesystem is corrupt or hello disk does not exist at boot time.</span></span> <span data-ttu-id="843e2-173">Ez a beállítás nélkül felmerülhet viselkedés leírtak [nem SSH tooLinux VM tooFSTAB hibák miatt](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).</span><span class="sxs-lookup"><span data-stu-id="843e2-173">Without this option, you may encounter behavior as described in [Cannot SSH tooLinux VM due tooFSTAB errors](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).</span></span>

    <span data-ttu-id="843e2-174">Most tesztelheti, hogy csatlakoztatva van-e a hello fájlrendszer megfelelően leválasztás, és ezután a fájlrendszer hello tudják, azaz hello példa csatlakoztatási pontot használ `/datadrive` hello létrehozott korábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="843e2-174">You can now test that hello file system is mounted properly by unmounting and then remounting hello file system, i.e. using hello example mount point `/datadrive` created in hello earlier steps:</span></span>

    ```bash
    sudo umount /datadrive
    sudo mount /datadrive
    ```

    <span data-ttu-id="843e2-175">Ha hello `mount` hoz létre hiba, ellenőrizze hello/etc/fstab fájl helytelen szintaxisú.</span><span class="sxs-lookup"><span data-stu-id="843e2-175">If hello `mount` command produces an error, check hello /etc/fstab file for correct syntax.</span></span> <span data-ttu-id="843e2-176">Ha további meghajtók és partíciók jönnek létre, adja meg azokat az/etc/fstab külön-külön is.</span><span class="sxs-lookup"><span data-stu-id="843e2-176">If additional data drives or partitions are created, enter them into /etc/fstab separately as well.</span></span>

    <span data-ttu-id="843e2-177">Hello meghajtó írható ellenőrizze a parancs segítségével:</span><span class="sxs-lookup"><span data-stu-id="843e2-177">Make hello drive writable by using this command:</span></span>

    ```bash
    sudo chmod go+w /datadrive
    ```

    > [!NOTE]
    > <span data-ttu-id="843e2-178">Ezt követően eltávolítása adatlemezt fstab szerkesztése nélkül támadó hello VM toofail tooboot.</span><span class="sxs-lookup"><span data-stu-id="843e2-178">Subsequently removing a data disk without editing fstab could cause hello VM toofail tooboot.</span></span> <span data-ttu-id="843e2-179">Ha ez gyakran előfordul, a legtöbb terjesztéseket adja meg vagy hello `nofail` és/vagy `nobootwait` fstab beállítások, amelyek lehetővé teszik a rendszer tooboot, még akkor is, ha hello lemezhiba toomount rendszerindítás.</span><span class="sxs-lookup"><span data-stu-id="843e2-179">If this is a common occurrence, most distributions provide either hello `nofail` and/or `nobootwait` fstab options that allow a system tooboot even if hello disk fails toomount at boot time.</span></span> <span data-ttu-id="843e2-180">A terjesztési dokumentációjában talál további információt ezekről a paraméterekről.</span><span class="sxs-lookup"><span data-stu-id="843e2-180">Consult your distribution's documentation for more information on these parameters.</span></span>

### <a name="trimunmap-support-for-linux-in-azure"></a><span data-ttu-id="843e2-181">Az Azure Linux vágást/UNMAP támogatása</span><span class="sxs-lookup"><span data-stu-id="843e2-181">TRIM/UNMAP support for Linux in Azure</span></span>
<span data-ttu-id="843e2-182">Egyes Linux kernelek vágást/UNMAP műveletek toodiscard támogatja a nem használt blokkok hello lemezen.</span><span class="sxs-lookup"><span data-stu-id="843e2-182">Some Linux kernels support TRIM/UNMAP operations toodiscard unused blocks on hello disk.</span></span> <span data-ttu-id="843e2-183">Ezek a műveletek elsősorban hasznosak standard tárolási tooinform Azure törölt lapok már nem érvényes, és is elvesznek.</span><span class="sxs-lookup"><span data-stu-id="843e2-183">These operations are primarily useful in standard storage tooinform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="843e2-184">Lapok elvetése költség mentheti, ha nagy fájlok létrehozása, majd törli őket.</span><span class="sxs-lookup"><span data-stu-id="843e2-184">Discarding pages can save cost if you create large files and then delete them.</span></span>

<span data-ttu-id="843e2-185">Két módon tooenable vágást támogatja a Linux virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="843e2-185">There are two ways tooenable TRIM support in your Linux VM.</span></span> <span data-ttu-id="843e2-186">A szokásos módon olvassa el a terjesztési hello ajánlott megközelítést alkalmazva:</span><span class="sxs-lookup"><span data-stu-id="843e2-186">As usual, consult your distribution for hello recommended approach:</span></span>

* <span data-ttu-id="843e2-187">Használjon hello `discard` lehetőség a csatlakoztatási `/etc/fstab`, például:</span><span class="sxs-lookup"><span data-stu-id="843e2-187">Use hello `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```

* <span data-ttu-id="843e2-188">Az egyes esetekben hello `discard` lehetőség is van a teljesítményre.</span><span class="sxs-lookup"><span data-stu-id="843e2-188">In some cases hello `discard` option may have performance implications.</span></span> <span data-ttu-id="843e2-189">Alternatív megoldásként futtathatja a hello `fstrim` manuálisan parancsot a parancssorból hello, vagy felveheti Ön is tooyour crontab toorun rendszeresen:</span><span class="sxs-lookup"><span data-stu-id="843e2-189">Alternatively, you can run hello `fstrim` command manually from hello command line, or add it tooyour crontab toorun regularly:</span></span>
  
    <span data-ttu-id="843e2-190">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="843e2-190">**Ubuntu**</span></span>
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    <span data-ttu-id="843e2-191">**RHEL vagy CentOS**</span><span class="sxs-lookup"><span data-stu-id="843e2-191">**RHEL/CentOS**</span></span>
  
    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a><span data-ttu-id="843e2-192">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="843e2-192">Troubleshooting</span></span>
[!INCLUDE [virtual-machines-linux-lunzero](../../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a><span data-ttu-id="843e2-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="843e2-193">Next Steps</span></span>
<span data-ttu-id="843e2-194">További a Linux virtuális gép használata a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="843e2-194">You can read more about using your Linux VM in hello following articles:</span></span>

* <span data-ttu-id="843e2-195">[Hogyan toolog tooa virtuális gépen futó Linux][Logon]</span><span class="sxs-lookup"><span data-stu-id="843e2-195">[How toolog on tooa virtual machine running Linux][Logon]</span></span>
* [<span data-ttu-id="843e2-196">Hogyan toodetach egy lemezt a Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="843e2-196">How toodetach a disk from a Linux virtual machine</span></span>](detach-disk.md)
* [<span data-ttu-id="843e2-197">Hello Azure CLI használata hello klasszikus telepítési modell</span><span class="sxs-lookup"><span data-stu-id="843e2-197">Using hello Azure CLI with hello Classic deployment model</span></span>](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [<span data-ttu-id="843e2-198">A Linux virtuális gép RAID konfigurálása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="843e2-198">Configure RAID on a Linux VM in Azure</span></span>](../configure-raid.md)
* [<span data-ttu-id="843e2-199">A Linux virtuális gép az Azure-ban LVM konfigurálása</span><span class="sxs-lookup"><span data-stu-id="843e2-199">Configure LVM on a Linux VM in Azure</span></span>](../configure-lvm.md)

<!--Link references-->
[Agent]:../agent-user-guide.md
[Logon]:../mac-create-ssh-keys.md
