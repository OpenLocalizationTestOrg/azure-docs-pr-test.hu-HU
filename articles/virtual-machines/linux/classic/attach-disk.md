---
title: "Lemez csatolása a Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan adatlemezt csatolni egy Linux virtuális gép a klasszikus telepítési modellt, és végezze el a lemez inicializálását, hogy készen álljon a használatra"
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
ms.openlocfilehash: 017ba7197e11c2b222082833d5acabb9e542b762
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a><span data-ttu-id="0a454-103">Hogyan lehet adatlemezt csatolni egy Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="0a454-103">How to Attach a Data Disk to a Linux Virtual Machine</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="0a454-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="0a454-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="0a454-105">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="0a454-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="0a454-106">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="0a454-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="0a454-107">Lásd: hogyan [erőforrás-kezelő telepítési modellel adatlemezzel](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0a454-107">See how to [attach a data disk using the Resource Manager deployment model](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="0a454-108">Csatolhat is üres és a lemezek, amelyek tartalmazzák az Azure virtuális gépek adatokat.</span><span class="sxs-lookup"><span data-stu-id="0a454-108">You can attach both empty disks and disks that contain data to your Azure VMs.</span></span> <span data-ttu-id="0a454-109">Mindkét típusú lemezek a .vhd fájlok találhatók az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="0a454-109">Both types of disks are .vhd files that reside in an Azure storage account.</span></span> <span data-ttu-id="0a454-110">Mint az olyan lemezek hozzáadása a Linux rendszerű számítógép, a lemez csatlakoztatása után kell inicializálni és formázni azt, hogy készen álljon a használatra.</span><span class="sxs-lookup"><span data-stu-id="0a454-110">As with adding any disk to a Linux machine, after you attach the disk you need to initialize and format it so it's ready for use.</span></span> <span data-ttu-id="0a454-111">Ez a cikk adatokat is üres és a lemezek már tartalmazó adatokat a virtuális gépeket, valamint majd inicializálni és formázni egy új lemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="0a454-111">This article details attaching both empty disks and disks already containing data to your VMs, as well as how to then initialize and format a new disk.</span></span>

> [!NOTE]
> <span data-ttu-id="0a454-112">Akkor célszerű egy vagy több különálló lemezek használatával egy virtuális gép adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="0a454-112">It's a best practice to use one or more separate disks to store a virtual machine's data.</span></span> <span data-ttu-id="0a454-113">Egy Azure virtuális gép létrehozásakor rendelkezik az operációs rendszer és egy ideiglenes lemezzel.</span><span class="sxs-lookup"><span data-stu-id="0a454-113">When you create an Azure virtual machine, it has an operating system disk and a temporary disk.</span></span> <span data-ttu-id="0a454-114">**Ne használja az ideiglenes lemez állandó adatok tárolására.**</span><span class="sxs-lookup"><span data-stu-id="0a454-114">**Do not use the temporary disk to store persistent data.**</span></span> <span data-ttu-id="0a454-115">Foglalja, csak az átmeneti tárolási biztosít.</span><span class="sxs-lookup"><span data-stu-id="0a454-115">As the name implies, it provides temporary storage only.</span></span> <span data-ttu-id="0a454-116">Kínál nincs redundancia vagy a biztonsági mentést, mert az nem található az Azure storage.</span><span class="sxs-lookup"><span data-stu-id="0a454-116">It offers no redundancy or backup because it doesn't reside in Azure storage.</span></span>
> <span data-ttu-id="0a454-117">Az ideiglenes lemez általában az Azure Linux ügynök által felügyelt, és automatikusan csatlakoztatva **/mnt és az erőforrások** (vagy **/mnt** Ubuntu képek).</span><span class="sxs-lookup"><span data-stu-id="0a454-117">The temporary disk is typically managed by the Azure Linux Agent and automatically mounted to **/mnt/resource** (or **/mnt** on Ubuntu images).</span></span> <span data-ttu-id="0a454-118">Másrészről, adatlemezt neve lehet a Linux kernel hasonlót `/dev/sdc`, és meg kell partíció formázása és csatlakoztatnia ehhez az erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="0a454-118">On the other hand, a data disk might be named by the Linux kernel something like `/dev/sdc`, and you need to partition, format, and mount this resource.</span></span> <span data-ttu-id="0a454-119">Tekintse meg a [Azure Linux ügynök felhasználói útmutató] [ Agent] részleteiről.</span><span class="sxs-lookup"><span data-stu-id="0a454-119">See the [Azure Linux Agent User Guide][Agent] for details.</span></span>
> 
> 

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a><span data-ttu-id="0a454-120">A Linux új adatlemezt inicializálása</span><span class="sxs-lookup"><span data-stu-id="0a454-120">Initialize a new data disk in Linux</span></span>
1. <span data-ttu-id="0a454-121">SSH-kapcsolatot a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="0a454-121">SSH to your VM.</span></span> <span data-ttu-id="0a454-122">További információkért lásd: [Linuxot futtató virtuális gép bejelentkezés][Logon].</span><span class="sxs-lookup"><span data-stu-id="0a454-122">For more information, see [How to log on to a virtual machine running Linux][Logon].</span></span>
2. <span data-ttu-id="0a454-123">A következő meg kell találnia az adatok lemez inicializálása eszközazonosító.</span><span class="sxs-lookup"><span data-stu-id="0a454-123">Next you need to find the device identifier for the data disk to initialize.</span></span> <span data-ttu-id="0a454-124">Ehhez két módja van:</span><span class="sxs-lookup"><span data-stu-id="0a454-124">There are two ways to do that:</span></span>
   
    <span data-ttu-id="0a454-125">a) Grep SCSI-eszközök a naplófájlokban, például a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0a454-125">a) Grep for SCSI devices in the logs, such as in the following command:</span></span>
   
    ```bash
    sudo grep SCSI /var/log/messages
    ```
   
    <span data-ttu-id="0a454-126">A legutóbbi Ubuntu azokat a terjesztéseket, akkor előfordulhat, hogy kell használnia `sudo grep SCSI /var/log/syslog` mert naplózásának `/var/log/messages` alapértelmezés szerint le lehetnek tiltva.</span><span class="sxs-lookup"><span data-stu-id="0a454-126">For recent Ubuntu distributions, you may need to use `sudo grep SCSI /var/log/syslog` because logging to `/var/log/messages` might be disabled by default.</span></span>
   
    <span data-ttu-id="0a454-127">A utolsó megjelenő üzeneteket hozzáadott adatlemez azonosítóját található.</span><span class="sxs-lookup"><span data-stu-id="0a454-127">You can find the identifier of the last data disk that was added in the messages that are displayed.</span></span>
   
    ![A lemez üzeneteket](./media/attach-disk/scsidisklog.png)
   
    <span data-ttu-id="0a454-129">VAGY</span><span class="sxs-lookup"><span data-stu-id="0a454-129">OR</span></span>
   
    <span data-ttu-id="0a454-130">b) használja a `lsscsi` parancs az eszközazonosítót megállapítása.</span><span class="sxs-lookup"><span data-stu-id="0a454-130">b) Use the `lsscsi` command to find out the device id.</span></span> <span data-ttu-id="0a454-131">Az `lsscsi` a `yum install lsscsi` (Red Hat-alapú disztribúciókon) vagy az `apt-get install lsscsi` (Debian-alapú disztribúciókon) használatával telepíthető.</span><span class="sxs-lookup"><span data-stu-id="0a454-131">`lsscsi` can be installed by either `yum install lsscsi` (on Red Hat based distributions) or `apt-get install lsscsi` (on Debian based distributions).</span></span> <span data-ttu-id="0a454-132">A lemez által keresett megtalálhatja a *lun* vagy **logikaiegység-szám**.</span><span class="sxs-lookup"><span data-stu-id="0a454-132">You can find the disk you are looking for by its *lun* or **logical unit number**.</span></span> <span data-ttu-id="0a454-133">Például a *lun* a csatolt a lemezek is könnyen látható `azure vm disk list <virtual-machine>` mint:</span><span class="sxs-lookup"><span data-stu-id="0a454-133">For example, the *lun* for the disks you attached can be easily seen from `azure vm disk list <virtual-machine>` as:</span></span>

    ```azurecli
    azure vm disk list myVM
    ```

    <span data-ttu-id="0a454-134">A kimenet a következő példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="0a454-134">The output is similar to the following:</span></span>

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
   
    <span data-ttu-id="0a454-135">Hasonlítsa össze ezeket az adatokat a kimenetét `lsscsi` az azonos virtuális gép mintavételhez:</span><span class="sxs-lookup"><span data-stu-id="0a454-135">Compare this data with the output of `lsscsi` for the same sample virtual machine:</span></span>
   
    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```
   
    <span data-ttu-id="0a454-136">Az utolsó helyen található minden egyes sorban levő rekord a *lun*.</span><span class="sxs-lookup"><span data-stu-id="0a454-136">The last number in the tuple in each row is the *lun*.</span></span> <span data-ttu-id="0a454-137">Lásd: `man lsscsi` további információt.</span><span class="sxs-lookup"><span data-stu-id="0a454-137">See `man lsscsi` for more information.</span></span>
3. <span data-ttu-id="0a454-138">A parancssorba írja be az eszköz létrehozásához a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0a454-138">At the prompt, type the following command to create your device:</span></span>
   
    ```bash
    sudo fdisk /dev/sdc
    ```

4. <span data-ttu-id="0a454-139">Amikor a rendszer kéri, írja be a  **n**  partíció létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0a454-139">When prompted, type **n** to create a partition.</span></span>

    ![Eszköz létrehozása](./media/attach-disk/fdisknewpartition.png)

5. <span data-ttu-id="0a454-141">Amikor a rendszer kéri, írja be a **p** legyen a partíció az elsődleges partíció számára.</span><span class="sxs-lookup"><span data-stu-id="0a454-141">When prompted, type **p** to make the partition the primary partition.</span></span> <span data-ttu-id="0a454-142">Típus **1** abba, hogy az első partíció, és adja meg az alapértelmezett érték a palack fogadásához, majd írja be.</span><span class="sxs-lookup"><span data-stu-id="0a454-142">Type **1** to make it the first partition, and then type enter to accept the default value for the cylinder.</span></span> <span data-ttu-id="0a454-143">Egyes rendszerek megmutatja az első és utolsó szektorok a palack alapértelmezett értékeit.</span><span class="sxs-lookup"><span data-stu-id="0a454-143">On some systems, it can show the default values of the first and the last sectors, instead of the cylinder.</span></span> <span data-ttu-id="0a454-144">Ha szeretné, fogadja el ezeket az alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="0a454-144">You can choose to accept these defaults.</span></span>

    ![A partíció](./media/attach-disk/fdisknewpartdetails.png)


6. <span data-ttu-id="0a454-146">Típus **p** kapcsolatos a lemez, amely alatt a részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="0a454-146">Type **p** to see the details about the disk that is being partitioned.</span></span>

    ![Lista fizikai lemezeinek adatai](./media/attach-disk/fdiskpartitiondetails.png)


7. <span data-ttu-id="0a454-148">Típus **w** a beállításokat a lemezre írni.</span><span class="sxs-lookup"><span data-stu-id="0a454-148">Type **w** to write the settings for the disk.</span></span>

    ![A lemezen változások írása](./media/attach-disk/fdiskwritedisk.png)

8. <span data-ttu-id="0a454-150">A fájlrendszer most már az új partícióra hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="0a454-150">Now you can create the file system on the new partition.</span></span> <span data-ttu-id="0a454-151">Az eszköz azonosítója a partíciószám hozzáfűzése (a következő példa `/dev/sdc1`).</span><span class="sxs-lookup"><span data-stu-id="0a454-151">Append the partition number to the device ID (in the following example `/dev/sdc1`).</span></span> <span data-ttu-id="0a454-152">A következő példa a /dev/sdc1 ext4 partíciót hoz létre:</span><span class="sxs-lookup"><span data-stu-id="0a454-152">The following example creates an ext4 partition on /dev/sdc1:</span></span>
   
    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
   
    ![-Fájl létrehozása](./media/attach-disk/mkfsext4.png)
   
   > [!NOTE]
   > <span data-ttu-id="0a454-154">SuSE Linux Enterprise 11 rendszerek csak ext4 fájlrendszerekhez támogatja a csak olvasási hozzáféréssel.</span><span class="sxs-lookup"><span data-stu-id="0a454-154">SuSE Linux Enterprise 11 systems only support read-only access for ext4 file systems.</span></span> <span data-ttu-id="0a454-155">Ezek a rendszerek javasoljuk, hogy az új fájlrendszer formátuma ext4 helyett ext3.</span><span class="sxs-lookup"><span data-stu-id="0a454-155">For these systems, it is recommended to format the new file system as ext3 rather than ext4.</span></span>

9. <span data-ttu-id="0a454-156">Ellenőrizze azt, hogy az új fájlrendszer, csatlakoztassa az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0a454-156">Make a directory to mount the new file system, as follows:</span></span>
   
    ```bash
    sudo mkdir /datadrive
    ```

10. <span data-ttu-id="0a454-157">Végül akkor is csatlakoztathatja a meghajtót, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0a454-157">Finally you can mount the drive, as follows:</span></span>
   
    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```
   
    <span data-ttu-id="0a454-158">Az adatlemez készen áll a használják **/datadrive**.</span><span class="sxs-lookup"><span data-stu-id="0a454-158">The data disk is now ready to use as **/datadrive**.</span></span>
   
    ![A következő könyvtár létrehozásakor, és csatlakoztassa a lemezt](./media/attach-disk/mkdirandmount.png)

11. <span data-ttu-id="0a454-160">Vegye fel az új meghajtó /etc/fstab:</span><span class="sxs-lookup"><span data-stu-id="0a454-160">Add the new drive to /etc/fstab:</span></span>
   
    <span data-ttu-id="0a454-161">Annak érdekében, hogy a meghajtó a rendszer újraindítása után automatikusan csatlakoztatni, hozzá kell adni a/etc/fstab fájlba.</span><span class="sxs-lookup"><span data-stu-id="0a454-161">To ensure the drive is remounted automatically after a reboot it must be added to the /etc/fstab file.</span></span> <span data-ttu-id="0a454-162">Ajánlott továbbá szolgál, hogy a UUID (univerzálisan egyedi azonosítója) /etc/fstab utal, csak az eszköz nevét (pl. /dev/sdc1) helyett.</span><span class="sxs-lookup"><span data-stu-id="0a454-162">In addition, it is highly recommended that the UUID (Universally Unique IDentifier) is used in /etc/fstab to refer to the drive rather than just the device name (i.e. /dev/sdc1).</span></span> <span data-ttu-id="0a454-163">A UUID használatával elkerülhető az éppen csatlakoztatva egy adott helyre, ha az operációs rendszer rendszerindítási és minden fennmaradó adatlemezt, majd hozzárendeli ezeket eszközazonosítók során észleli a lemezhiba helytelen lemezkonfigurációt.</span><span class="sxs-lookup"><span data-stu-id="0a454-163">Using the UUID avoids the incorrect disk being mounted to a given location if the OS detects a disk error during boot and any remaining data disks then being assigned those device IDs.</span></span> <span data-ttu-id="0a454-164">Az új meghajtó UUID megkereséséhez használja a **blkid** segédprogram:</span><span class="sxs-lookup"><span data-stu-id="0a454-164">To find the UUID of the new drive, you can use the **blkid** utility:</span></span>
   
    ```bash
    sudo -i blkid
    ```
   
    <span data-ttu-id="0a454-165">A kimeneti az alábbihoz hasonlít:</span><span class="sxs-lookup"><span data-stu-id="0a454-165">The output looks similar to the following example:</span></span>
   
    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

    > [!NOTE]
    > <span data-ttu-id="0a454-166">Nem megfelelő módosítása a **/etc/fstab** fájl meghiúsulását eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="0a454-166">Improperly editing the **/etc/fstab** file could result in an unbootable system.</span></span> <span data-ttu-id="0a454-167">Ha nem ismeri, tekintse meg a telepítési dokumentációban talál információkat arról a fájl megfelelően szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="0a454-167">If unsure, refer to the distribution's documentation for information on how to properly edit this file.</span></span> <span data-ttu-id="0a454-168">Ajánlott továbbá, hogy létrejött-e a fájl biztonsági másolata /etc/fstab szerkesztése előtt.</span><span class="sxs-lookup"><span data-stu-id="0a454-168">It is also recommended that a backup of the /etc/fstab file is created before editing.</span></span>

    <span data-ttu-id="0a454-169">Ezután nyissa meg a **/etc/fstab** fájlt egy szövegszerkesztőben:</span><span class="sxs-lookup"><span data-stu-id="0a454-169">Next, open the **/etc/fstab** file in a text editor:</span></span>

    ```bash
    sudo vi /etc/fstab
    ```

    <span data-ttu-id="0a454-170">A jelen példában használjuk a UUID az új **/dev/sdc1** eszköz, amely az előző lépést, és a csatlakoztatási pont jött létre **/datadrive**.</span><span class="sxs-lookup"><span data-stu-id="0a454-170">In this example, we use the UUID value for the new **/dev/sdc1** device that was created in the previous steps, and the mountpoint **/datadrive**.</span></span> <span data-ttu-id="0a454-171">Adja hozzá az alábbi végének a **/etc/fstab** fájlt:</span><span class="sxs-lookup"><span data-stu-id="0a454-171">Add the following line to the end of the **/etc/fstab** file:</span></span>

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
    ```

    <span data-ttu-id="0a454-172">Vagy a SuSE Linux-alapú rendszereken esetleg némileg eltérő formátumának a használatára:</span><span class="sxs-lookup"><span data-stu-id="0a454-172">Or, on systems based on SuSE Linux you may need to use a slightly different format:</span></span>

    ```sh
    /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    ```

    > [!NOTE]
    > <span data-ttu-id="0a454-173">A `nofail` lehetőség biztosítja, hogy a virtuális gép elindul, még akkor is, ha a fájlrendszer sérült, vagy a lemez nem létezik a következő rendszerindításkor.</span><span class="sxs-lookup"><span data-stu-id="0a454-173">The `nofail` option ensures that the VM starts even if the filesystem is corrupt or the disk does not exist at boot time.</span></span> <span data-ttu-id="0a454-174">Ez a beállítás nélkül felmerülhet viselkedés leírtak [nem SSH Linux virtuális gép között az FSTAB hibák miatt](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).</span><span class="sxs-lookup"><span data-stu-id="0a454-174">Without this option, you may encounter behavior as described in [Cannot SSH to Linux VM due to FSTAB errors](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).</span></span>

    <span data-ttu-id="0a454-175">Most tesztelheti, hogy a fájlrendszer által leválasztás és majd tudják azaz a fájlrendszerben, megfelelően csatlakoztatott</span><span class="sxs-lookup"><span data-stu-id="0a454-175">You can now test that the file system is mounted properly by unmounting and then remounting the file system, i.e.</span></span> <span data-ttu-id="0a454-176">a példa csatlakoztatási pontot `/datadrive` a korábbi lépésekben létrehozott:</span><span class="sxs-lookup"><span data-stu-id="0a454-176">using the example mount point `/datadrive` created in the earlier steps:</span></span>

    ```bash
    sudo umount /datadrive
    sudo mount /datadrive
    ```

    <span data-ttu-id="0a454-177">Ha a `mount` hibát hoz létre, jelölje be a helyes szintaxis/etc/fstab fájlt.</span><span class="sxs-lookup"><span data-stu-id="0a454-177">If the `mount` command produces an error, check the /etc/fstab file for correct syntax.</span></span> <span data-ttu-id="0a454-178">Ha további meghajtók és partíciók jönnek létre, adja meg azokat az/etc/fstab külön-külön is.</span><span class="sxs-lookup"><span data-stu-id="0a454-178">If additional data drives or partitions are created, enter them into /etc/fstab separately as well.</span></span>

    <span data-ttu-id="0a454-179">A meghajtó írható ellenőrizze a parancs segítségével:</span><span class="sxs-lookup"><span data-stu-id="0a454-179">Make the drive writable by using this command:</span></span>

    ```bash
    sudo chmod go+w /datadrive
    ```

    > [!NOTE]
    > <span data-ttu-id="0a454-180">Ezt követően eltávolítása adatlemezt fstab szerkesztése nélkül, hogy a virtuális gépek a rendszerindítás.</span><span class="sxs-lookup"><span data-stu-id="0a454-180">Subsequently removing a data disk without editing fstab could cause the VM to fail to boot.</span></span> <span data-ttu-id="0a454-181">Ha ez gyakran előfordul, a legtöbb terjesztéseket biztosítanak, vagy a `nofail` és/vagy `nobootwait` fstab beállítások, amelyek lehetővé teszik a rendszert még akkor is, ha a lemez nem tudja csatlakoztatni rendszerindító idő.</span><span class="sxs-lookup"><span data-stu-id="0a454-181">If this is a common occurrence, most distributions provide either the `nofail` and/or `nobootwait` fstab options that allow a system to boot even if the disk fails to mount at boot time.</span></span> <span data-ttu-id="0a454-182">A terjesztési dokumentációjában talál további információt ezekről a paraméterekről.</span><span class="sxs-lookup"><span data-stu-id="0a454-182">Consult your distribution's documentation for more information on these parameters.</span></span>

### <a name="trimunmap-support-for-linux-in-azure"></a><span data-ttu-id="0a454-183">Az Azure Linux vágást/UNMAP támogatása</span><span class="sxs-lookup"><span data-stu-id="0a454-183">TRIM/UNMAP support for Linux in Azure</span></span>
<span data-ttu-id="0a454-184">Egyes Linux kernelek támogatja a vágás/UNMAP műveleteket elveti a nem használt blokkok a lemezen.</span><span class="sxs-lookup"><span data-stu-id="0a454-184">Some Linux kernels support TRIM/UNMAP operations to discard unused blocks on the disk.</span></span> <span data-ttu-id="0a454-185">Ezek a műveletek elsősorban hasznosak standard tárolási tájékoztatja Azure törölt lapok már nem érvényesek, és is elvesznek.</span><span class="sxs-lookup"><span data-stu-id="0a454-185">These operations are primarily useful in standard storage to inform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="0a454-186">Lapok elvetése költség mentheti, ha nagy fájlok létrehozása, majd törli őket.</span><span class="sxs-lookup"><span data-stu-id="0a454-186">Discarding pages can save cost if you create large files and then delete them.</span></span>

<span data-ttu-id="0a454-187">Két módon vágást engedélyezése támogatja a Linux virtuális gép van.</span><span class="sxs-lookup"><span data-stu-id="0a454-187">There are two ways to enable TRIM support in your Linux VM.</span></span> <span data-ttu-id="0a454-188">A szokásos módon olvassa el a terjesztési az ajánlott módszer:</span><span class="sxs-lookup"><span data-stu-id="0a454-188">As usual, consult your distribution for the recommended approach:</span></span>

* <span data-ttu-id="0a454-189">Használja a `discard` lehetőség a csatlakoztatási `/etc/fstab`, például:</span><span class="sxs-lookup"><span data-stu-id="0a454-189">Use the `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```

* <span data-ttu-id="0a454-190">Bizonyos esetekben a `discard` lehetőség is van a teljesítményre.</span><span class="sxs-lookup"><span data-stu-id="0a454-190">In some cases the `discard` option may have performance implications.</span></span> <span data-ttu-id="0a454-191">Alternatív megoldásként futtathatja a `fstrim` manuálisan parancsot a parancssorból, vagy adja hozzá a crontab rendszeresen futtatásához:</span><span class="sxs-lookup"><span data-stu-id="0a454-191">Alternatively, you can run the `fstrim` command manually from the command line, or add it to your crontab to run regularly:</span></span>
  
    <span data-ttu-id="0a454-192">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="0a454-192">**Ubuntu**</span></span>
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    <span data-ttu-id="0a454-193">**RHEL vagy CentOS**</span><span class="sxs-lookup"><span data-stu-id="0a454-193">**RHEL/CentOS**</span></span>
  
    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a><span data-ttu-id="0a454-194">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="0a454-194">Troubleshooting</span></span>
[!INCLUDE [virtual-machines-linux-lunzero](../../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a><span data-ttu-id="0a454-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0a454-195">Next Steps</span></span>
<span data-ttu-id="0a454-196">További információk a Linux virtuális gép használata a következő cikkekben:</span><span class="sxs-lookup"><span data-stu-id="0a454-196">You can read more about using your Linux VM in the following articles:</span></span>

* <span data-ttu-id="0a454-197">[Bejelentkezés a Linux rendszerű virtuális gép][Logon]</span><span class="sxs-lookup"><span data-stu-id="0a454-197">[How to log on to a virtual machine running Linux][Logon]</span></span>
* [<span data-ttu-id="0a454-198">Útmutató a lemezt leválasztani a Linux virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="0a454-198">How to detach a disk from a Linux virtual machine</span></span>](detach-disk.md)
* [<span data-ttu-id="0a454-199">A klasszikus telepítési modellt az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="0a454-199">Using the Azure CLI with the Classic deployment model</span></span>](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [<span data-ttu-id="0a454-200">A Linux virtuális gép RAID konfigurálása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="0a454-200">Configure RAID on a Linux VM in Azure</span></span>](../configure-raid.md)
* [<span data-ttu-id="0a454-201">A Linux virtuális gép az Azure-ban LVM konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0a454-201">Configure LVM on a Linux VM in Azure</span></span>](../configure-lvm.md)

<!--Link references-->
[Agent]:../agent-user-guide.md
[Logon]:../mac-create-ssh-keys.md
