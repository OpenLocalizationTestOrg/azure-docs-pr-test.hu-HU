---
title: "a lemez tooLinux VM használatával aaaAdd hello Azure CLI |} Microsoft Docs"
description: "Ismerje meg, hogy tooadd hello Azure CLI 1.0-s és 2.0 állandó lemez tooyour Linux virtuális gép."
keywords: "Linux virtuális gép, erőforrás lemez hozzáadása"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 3005a066-7a84-4dc5-bdaa-574c75e6e411
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 02/02/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0dc5236be62d96b70dd47a7f621f626a037e22aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-disk-tooa-linux-vm"></a><span data-ttu-id="42c9f-104">A lemez tooa Linux virtuális gép hozzáadása</span><span class="sxs-lookup"><span data-stu-id="42c9f-104">Add a disk tooa Linux VM</span></span>
<span data-ttu-id="42c9f-105">Ez a cikk bemutatja, hogyan egy állandó tooattach lemez tooyour VM, hogy az adatok - megőrizhető akkor is, ha a virtuális gép van újra kiépíteni toomaintenance vagy átméretezése miatt.</span><span class="sxs-lookup"><span data-stu-id="42c9f-105">This article shows how tooattach a persistent disk tooyour VM so that you can preserve your data - even if your VM is reprovisioned due toomaintenance or resizing.</span></span> 

## <a name="quick-commands"></a><span data-ttu-id="42c9f-106">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="42c9f-106">Quick Commands</span></span>
<span data-ttu-id="42c9f-107">Példa rendeli következő hello egy `50`GB szabad toohello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="42c9f-107">hello following example attaches a `50`GB disk toohello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

<span data-ttu-id="42c9f-108">toouse által kezelt lemezeken:</span><span class="sxs-lookup"><span data-stu-id="42c9f-108">toouse managed disks:</span></span>

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

<span data-ttu-id="42c9f-109">nem felügyelt toouse lemezek:</span><span class="sxs-lookup"><span data-stu-id="42c9f-109">toouse unmanaged disks:</span></span>

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```

## <a name="attach-a-managed-disk"></a><span data-ttu-id="42c9f-110">Egy kezelt lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="42c9f-110">Attach a managed disk</span></span>

<span data-ttu-id="42c9f-111">Felügyelt lemezek használata lehetővé teszi a virtuális gépek és a lemezek toofocus anélkül, hogy Azure Storage-fiókokat.</span><span class="sxs-lookup"><span data-stu-id="42c9f-111">Using managed disks enables you toofocus on your VMs and their disks without worrying about Azure Storage accounts.</span></span> <span data-ttu-id="42c9f-112">Gyorsan hozhat létre, és felügyelt lemezt csatlakoztatni tooa VM használatával hello azonos Azure-erőforráscsoportot, vagy létrehozhat bármely lemezek számát, majd csatolja.</span><span class="sxs-lookup"><span data-stu-id="42c9f-112">You can quickly create and attach a managed disk tooa VM using hello same Azure resource group, or you can create any number of disks and then attach them.</span></span>


### <a name="attach-a-new-disk-tooa-vm"></a><span data-ttu-id="42c9f-113">Új lemez tooa VM csatolása</span><span class="sxs-lookup"><span data-stu-id="42c9f-113">Attach a new disk tooa VM</span></span>

<span data-ttu-id="42c9f-114">Ha csak egy új lemezt a virtuális gépen, használhatja a hello `az vm disk attach` parancsot.</span><span class="sxs-lookup"><span data-stu-id="42c9f-114">If you just need a new disk on your VM, you can use hello `az vm disk attach` command.</span></span>

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

### <a name="attach-an-existing-disk"></a><span data-ttu-id="42c9f-115">Meglévő lemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="42c9f-115">Attach an existing disk</span></span> 

<span data-ttu-id="42c9f-116">Sok esetben már létrehozott csatolni.</span><span class="sxs-lookup"><span data-stu-id="42c9f-116">In many cases you attach disks that have already been created.</span></span> <span data-ttu-id="42c9f-117">Először a hello lemezazonosítót található, és akkor továbbítja az adott toohello `az vm disk attach` parancsot.</span><span class="sxs-lookup"><span data-stu-id="42c9f-117">You first find hello disk id and then pass that toohello `az vm disk attach` command.</span></span> <span data-ttu-id="42c9f-118">hello alábbi példa egy lemezt használ létre `az disk create -g myResourceGroup -n myDataDisk --size-gb 50`.</span><span class="sxs-lookup"><span data-stu-id="42c9f-118">hello following example uses a disk created with `az disk create -g myResourceGroup -n myDataDisk --size-gb 50`.</span></span>

```azurecli
# find hello disk id
diskId=$(az disk show -g myResourceGroup -n myDataDisk --query 'id' -o tsv)
az vm disk attach -g myResourceGroup --vm-name myVM --disk $diskId
```

<span data-ttu-id="42c9f-119">hello kimeneti alábbihoz hasonló hello (használhatja hello `-o table` tooany tooformat hello parancskimenet a beállítás):</span><span class="sxs-lookup"><span data-stu-id="42c9f-119">hello output looks something like hello following (you can use hello `-o table` option tooany command tooformat hello output in ):</span></span>

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Empty",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 50,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/rasquill-script/providers/Microsoft.Compute/disks/myDataDisk",
  "location": "westus",
  "name": "myDataDisk",
  "osType": null,
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-02T23:35:47.708082+00:00",
  "type": "Microsoft.Compute/disks"
}
```


## <a name="attach-an-unmanaged-disk"></a><span data-ttu-id="42c9f-120">Nem kezelt lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="42c9f-120">Attach an unmanaged disk</span></span>

<span data-ttu-id="42c9f-121">Új lemez csatolása kész, ha nincs ellenére lemez létrehozásához a hello ugyanazt a tárfiókot, mint a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="42c9f-121">Attaching a new disk is quick if you do not mind creating a disk in hello same storage account as your VM.</span></span> <span data-ttu-id="42c9f-122">Típus `azure vm disk attach-new` toocreate és egy új GB lemezt csatolni a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="42c9f-122">Type `azure vm disk attach-new` toocreate and attach a new GB disk for your VM.</span></span> <span data-ttu-id="42c9f-123">Ha explicit módon nem talál egy tárfiókot, a lemez kerül hello azonos az operációsrendszer-lemez tartalmazó tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="42c9f-123">If you do not explicitly identify a storage account, any disk you create is placed in hello same storage account where your OS disk resides.</span></span> <span data-ttu-id="42c9f-124">Példa rendeli következő hello egy `50`GB szabad toohello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="42c9f-124">hello following example attaches a `50`GB disk toohello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```

## <a name="connect-toohello-linux-vm-toomount-hello-new-disk"></a><span data-ttu-id="42c9f-125">Csatlakozás toohello Linux virtuális gép toomount hello új lemez</span><span class="sxs-lookup"><span data-stu-id="42c9f-125">Connect toohello Linux VM toomount hello new disk</span></span>
> [!NOTE]
> <span data-ttu-id="42c9f-126">Ez a témakör tooa Virtuálisgép kapcsolódik felhasználónevek és jelszavak használatával.</span><span class="sxs-lookup"><span data-stu-id="42c9f-126">This topic connects tooa VM using usernames and passwords.</span></span> <span data-ttu-id="42c9f-127">a virtuális Gépet, a toouse nyilvános és titkos kulcspárokat toocommunicate lásd [hogyan tooUse Azure Linux SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="42c9f-127">toouse public and private key pairs toocommunicate with your VM, see [How tooUse SSH with Linux on Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 
> 
> 

<span data-ttu-id="42c9f-128">TooSSH van szüksége az Azure virtuális gép toopartition formázása, és csatlakoztassa az új lemezt, a Linux virtuális gép használhassa azt.</span><span class="sxs-lookup"><span data-stu-id="42c9f-128">You need tooSSH into your Azure VM toopartition, format, and mount your new disk so your Linux VM can use it.</span></span> <span data-ttu-id="42c9f-129">Ha még nem ismeri a Kapcsolódás a **ssh**, hello parancs felsorolásszerűen hello űrlap `ssh <username>@<FQDNofAzureVM> -p <hello ssh port>`, és a következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="42c9f-129">If you're not familiar with connecting with **ssh**, hello command takes hello form `ssh <username>@<FQDNofAzureVM> -p <hello ssh port>`, and looks like hello following:</span></span>

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com -p 22
```

<span data-ttu-id="42c9f-130">Kimenet</span><span class="sxs-lookup"><span data-stu-id="42c9f-130">Output</span></span>

```bash
hello authenticity of host 'mypublicdns.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want toocontinue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) toohello list of known hosts.
ops@mypublicdns.westus.cloudapp.azure.com's password:
Welcome tooUbuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

hello programs included with hello Ubuntu system are free software;
hello exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, toohello extent permitted by
applicable law.

ops@myVM:~$
```

<span data-ttu-id="42c9f-131">Most, hogy csatlakozott tooyour VM, készen áll a lemez tooattach.</span><span class="sxs-lookup"><span data-stu-id="42c9f-131">Now that you're connected tooyour VM, you're ready tooattach a disk.</span></span>  <span data-ttu-id="42c9f-132">Először megkeresi a hello lemez használatával `dmesg | grep SCSI` (hello módszert használja az új lemez eltérhetnek toodiscover).</span><span class="sxs-lookup"><span data-stu-id="42c9f-132">First find hello disk, using `dmesg | grep SCSI` (hello method you use toodiscover your new disk may vary).</span></span> <span data-ttu-id="42c9f-133">Ebben az esetben az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="42c9f-133">In this case, it looks something like:</span></span>

```bash
dmesg | grep SCSI
```

<span data-ttu-id="42c9f-134">Kimenet</span><span class="sxs-lookup"><span data-stu-id="42c9f-134">Output</span></span>

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

<span data-ttu-id="42c9f-135">Ez a témakör hello esetben hello és `sdc` lemez hello, amelyet azt szeretnénk, ha.</span><span class="sxs-lookup"><span data-stu-id="42c9f-135">and in hello case of this topic, hello `sdc` disk is hello one that we want.</span></span> <span data-ttu-id="42c9f-136">Partíció rendelkező hello lemez `sudo fdisk /dev/sdc` – feltételezve, hogy az eset hello lemez volt `sdc`, és legyen egy elsődleges szabad a %1 partíció, és fogadja el hello többi alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="42c9f-136">Now partition hello disk with `sudo fdisk /dev/sdc` -- assuming that in your case hello disk was `sdc`, and make it a primary disk on partition 1, and accept hello other defaults.</span></span>

```bash
sudo fdisk /dev/sdc
```

<span data-ttu-id="42c9f-137">Kimenet</span><span class="sxs-lookup"><span data-stu-id="42c9f-137">Output</span></span>

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide toowrite them.
After that, of course, hello previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

<span data-ttu-id="42c9f-138">A partíció hello beírásával `p` hello parancssorba:</span><span class="sxs-lookup"><span data-stu-id="42c9f-138">Create hello partition by typing `p` at hello prompt:</span></span>

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
hello partition table has been altered!

Calling ioctl() toore-read partition table.
Syncing disks.
```

<span data-ttu-id="42c9f-139">Írási és olvasási egy fájl rendszerpartíciót toohello hello segítségével **mkfs** parancs, a fájlrendszer típusát és hello név megadásával.</span><span class="sxs-lookup"><span data-stu-id="42c9f-139">And write a file system toohello partition by using hello **mkfs** command, specifying your filesystem type and hello device name.</span></span> <span data-ttu-id="42c9f-140">Ebben a témakörben használunk `ext4` és `/dev/sdc1` a fent:</span><span class="sxs-lookup"><span data-stu-id="42c9f-140">In this topic, we're using `ext4` and `/dev/sdc1` from above:</span></span>

```bash
sudo mkfs -t ext4 /dev/sdc1
```

<span data-ttu-id="42c9f-141">Kimenet</span><span class="sxs-lookup"><span data-stu-id="42c9f-141">Output</span></span>

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for hello super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

<span data-ttu-id="42c9f-142">Most azt létre directory toomount hello fájl rendszer `mkdir`:</span><span class="sxs-lookup"><span data-stu-id="42c9f-142">Now we create a directory toomount hello file system using `mkdir`:</span></span>

```bash
sudo mkdir /datadrive
```

<span data-ttu-id="42c9f-143">Hello könyvtár használatával csatlakoztatja és `mount`:</span><span class="sxs-lookup"><span data-stu-id="42c9f-143">And you mount hello directory using `mount`:</span></span>

```bash
sudo mount /dev/sdc1 /datadrive
```

<span data-ttu-id="42c9f-144">hello adatlemeze most már készen áll a toouse, `/datadrive`.</span><span class="sxs-lookup"><span data-stu-id="42c9f-144">hello data disk is now ready toouse as `/datadrive`.</span></span>

```bash
ls
```

<span data-ttu-id="42c9f-145">Kimenet</span><span class="sxs-lookup"><span data-stu-id="42c9f-145">Output</span></span>

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

<span data-ttu-id="42c9f-146">tooensure hello meghajtót csatlakoztatni automatikusan, kell legyen újraindítás toohello /etc/fstab fájl hozzáadása után.</span><span class="sxs-lookup"><span data-stu-id="42c9f-146">tooensure hello drive is remounted automatically after a reboot it must be added toohello /etc/fstab file.</span></span> <span data-ttu-id="42c9f-147">Emellett javasoljuk, hogy hello UUID (univerzálisan egyedi azonosítója) használatos etc/fstab toorefer toohello meghajtó helyett csak hello eszköz neve (például `/dev/sdc1`).</span><span class="sxs-lookup"><span data-stu-id="42c9f-147">In addition, it is highly recommended that hello UUID (Universally Unique IDentifier) is used in /etc/fstab toorefer toohello drive rather than just hello device name (such as, `/dev/sdc1`).</span></span> <span data-ttu-id="42c9f-148">Hello OS lemezhiba rendszerindítás során észleli, ha hello UUID használatával elkerülhető hello helytelen lemezkonfigurációt alatt csatlakoztatott tooa megadott helyre.</span><span class="sxs-lookup"><span data-stu-id="42c9f-148">If hello OS detects a disk error during boot, using hello UUID avoids hello incorrect disk being mounted tooa given location.</span></span> <span data-ttu-id="42c9f-149">Adatlemezek fennmaradó volna lehet hozzárendelni ezeket azonos eszközazonosítókat.</span><span class="sxs-lookup"><span data-stu-id="42c9f-149">Remaining data disks would then be assigned those same device IDs.</span></span> <span data-ttu-id="42c9f-150">toofind hello UUID hello új meghajtó, használja a hello **blkid** segédprogram:</span><span class="sxs-lookup"><span data-stu-id="42c9f-150">toofind hello UUID of hello new drive, use hello **blkid** utility:</span></span>

```bash
sudo -i blkid
```

<span data-ttu-id="42c9f-151">hello kimeneti hasonló toohello következő néz ki:</span><span class="sxs-lookup"><span data-stu-id="42c9f-151">hello output looks similar toohello following:</span></span>

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

> [!NOTE]
> <span data-ttu-id="42c9f-152">Nem megfelelően szerkesztése hello **/etc/fstab** fájl meghiúsulását eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="42c9f-152">Improperly editing hello **/etc/fstab** file could result in an unbootable system.</span></span> <span data-ttu-id="42c9f-153">Ha nem ismeri, tekintse meg a toohello terjesztési dokumentációját olvashat, hogyan tooproperly szerkesztheti a fájlt.</span><span class="sxs-lookup"><span data-stu-id="42c9f-153">If unsure, refer toohello distribution's documentation for information on how tooproperly edit this file.</span></span> <span data-ttu-id="42c9f-154">Ajánlott továbbá, hogy létrejött-e a fájl biztonsági másolata hello /etc/fstab szerkesztése előtt.</span><span class="sxs-lookup"><span data-stu-id="42c9f-154">It is also recommended that a backup of hello /etc/fstab file is created before editing.</span></span>
> 
> 

<span data-ttu-id="42c9f-155">Ezután nyissa meg a hello **/etc/fstab** fájlt egy szövegszerkesztőben:</span><span class="sxs-lookup"><span data-stu-id="42c9f-155">Next, open hello **/etc/fstab** file in a text editor:</span></span>

```bash
sudo vi /etc/fstab
```

<span data-ttu-id="42c9f-156">A jelen példában használjuk hello UUID hello az új **/dev/sdc1** hello előző lépést, és hello csatlakozásipont létrehozott eszköz **/datadrive**.</span><span class="sxs-lookup"><span data-stu-id="42c9f-156">In this example, we use hello UUID value for hello new **/dev/sdc1** device that was created in hello previous steps, and hello mountpoint **/datadrive**.</span></span> <span data-ttu-id="42c9f-157">Adja hozzá a következő sor toohello vége hello hello **/etc/fstab** fájlt:</span><span class="sxs-lookup"><span data-stu-id="42c9f-157">Add hello following line toohello end of hello **/etc/fstab** file:</span></span>

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

> [!NOTE]
> <span data-ttu-id="42c9f-158">Később eltávolítása adatlemezt fstab szerkesztése nélkül támadó hello VM toofail tooboot.</span><span class="sxs-lookup"><span data-stu-id="42c9f-158">Later removing a data disk without editing fstab could cause hello VM toofail tooboot.</span></span> <span data-ttu-id="42c9f-159">A legtöbb azokat a terjesztéseket, adja meg vagy hello `nofail` és/vagy `nobootwait` fstab beállítások.</span><span class="sxs-lookup"><span data-stu-id="42c9f-159">Most distributions provide either hello `nofail` and/or `nobootwait` fstab options.</span></span> <span data-ttu-id="42c9f-160">Ezek a beállítások engedélyezése a rendszer tooboot, még akkor is, ha hello lemezhiba toomount rendszerindítás.</span><span class="sxs-lookup"><span data-stu-id="42c9f-160">These options allow a system tooboot even if hello disk fails toomount at boot time.</span></span> <span data-ttu-id="42c9f-161">A terjesztési dokumentációjában talál további információt ezekről a paraméterekről.</span><span class="sxs-lookup"><span data-stu-id="42c9f-161">Consult your distribution's documentation for more information on these parameters.</span></span>
> 
> <span data-ttu-id="42c9f-162">Hello **nofail** lehetőség biztosítja, hogy virtuális gép elindul, még akkor is, ha hello fájlrendszer sérült, vagy hello lemez nem létezik a rendszerindítás hello.</span><span class="sxs-lookup"><span data-stu-id="42c9f-162">hello **nofail** option ensures that hello VM starts even if hello filesystem is corrupt or hello disk does not exist at boot time.</span></span> <span data-ttu-id="42c9f-163">Ez a beállítás nélkül felmerülhet viselkedés leírtak [nem SSH tooLinux VM tooFSTAB hibák miatt](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)</span><span class="sxs-lookup"><span data-stu-id="42c9f-163">Without this option, you may encounter behavior as described in [Cannot SSH tooLinux VM due tooFSTAB errors](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)</span></span>

### <a name="trimunmap-support-for-linux-in-azure"></a><span data-ttu-id="42c9f-164">Az Azure Linux vágást/UNMAP támogatása</span><span class="sxs-lookup"><span data-stu-id="42c9f-164">TRIM/UNMAP support for Linux in Azure</span></span>
<span data-ttu-id="42c9f-165">Egyes Linux kernelek vágást/UNMAP műveletek toodiscard támogatja a nem használt blokkok hello lemezen.</span><span class="sxs-lookup"><span data-stu-id="42c9f-165">Some Linux kernels support TRIM/UNMAP operations toodiscard unused blocks on hello disk.</span></span> <span data-ttu-id="42c9f-166">Ez elsősorban fontos a standard szintű tárolást tooinform Azure törölt lapok már nem érvényes, és is elvesznek.</span><span class="sxs-lookup"><span data-stu-id="42c9f-166">This is primarily useful in standard storage tooinform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="42c9f-167">Ez költség mentheti, ha nagy fájlok létrehozása, majd törli őket.</span><span class="sxs-lookup"><span data-stu-id="42c9f-167">This can save cost if you create large files and then delete them.</span></span>

<span data-ttu-id="42c9f-168">Két módon tooenable vágást támogatja a Linux virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="42c9f-168">There are two ways tooenable TRIM support in your Linux VM.</span></span> <span data-ttu-id="42c9f-169">A szokásos módon olvassa el a terjesztési hello ajánlott megközelítést alkalmazva:</span><span class="sxs-lookup"><span data-stu-id="42c9f-169">As usual, consult your distribution for hello recommended approach:</span></span>

* <span data-ttu-id="42c9f-170">Használjon hello `discard` lehetőség a csatlakoztatási `/etc/fstab`, például:</span><span class="sxs-lookup"><span data-stu-id="42c9f-170">Use hello `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```
* <span data-ttu-id="42c9f-171">Az egyes esetekben hello `discard` lehetőség is van a teljesítményre.</span><span class="sxs-lookup"><span data-stu-id="42c9f-171">In some cases hello `discard` option may have performance implications.</span></span> <span data-ttu-id="42c9f-172">Alternatív megoldásként futtathatja a hello `fstrim` manuálisan parancsot a parancssorból hello, vagy felveheti Ön is tooyour crontab toorun rendszeresen:</span><span class="sxs-lookup"><span data-stu-id="42c9f-172">Alternatively, you can run hello `fstrim` command manually from hello command line, or add it tooyour crontab toorun regularly:</span></span>
  
    <span data-ttu-id="42c9f-173">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="42c9f-173">**Ubuntu**</span></span>
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    <span data-ttu-id="42c9f-174">**RHEL vagy CentOS**</span><span class="sxs-lookup"><span data-stu-id="42c9f-174">**RHEL/CentOS**</span></span>

    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a><span data-ttu-id="42c9f-175">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="42c9f-175">Troubleshooting</span></span>
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a><span data-ttu-id="42c9f-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42c9f-176">Next Steps</span></span>
* <span data-ttu-id="42c9f-177">Ne feledje, hogy az új lemez nincs elérhető toohello VM Ha csak lehet írni, hogy információkat tooyour újraindítja [fstab](http://en.wikipedia.org/wiki/Fstab) fájlt.</span><span class="sxs-lookup"><span data-stu-id="42c9f-177">Remember, that your new disk is not available toohello VM if it reboots unless you write that information tooyour [fstab](http://en.wikipedia.org/wiki/Fstab) file.</span></span>
* <span data-ttu-id="42c9f-178">a Linux virtuális gép tooensure konfigurációja helytelen, tekintse át hello [a Linux-gépek teljesítményének optimalizálásához](optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) javaslatok.</span><span class="sxs-lookup"><span data-stu-id="42c9f-178">tooensure your Linux VM is configured correctly, review hello [Optimize your Linux machine performance](optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) recommendations.</span></span>
* <span data-ttu-id="42c9f-179">Bontsa ki a tárolási kapacitás további lemezek felvételével és [RAID konfigurálása](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="42c9f-179">Expand your storage capacity by adding additional disks and [configure RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for additional performance.</span></span>

