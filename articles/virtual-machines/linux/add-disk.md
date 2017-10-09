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
# <a name="add-a-disk-tooa-linux-vm"></a>A lemez tooa Linux virtuális gép hozzáadása
Ez a cikk bemutatja, hogyan egy állandó tooattach lemez tooyour VM, hogy az adatok - megőrizhető akkor is, ha a virtuális gép van újra kiépíteni toomaintenance vagy átméretezése miatt. 

## <a name="quick-commands"></a>Gyors parancsok
Példa rendeli következő hello egy `50`GB szabad toohello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:

toouse által kezelt lemezeken:

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

nem felügyelt toouse lemezek:

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```

## <a name="attach-a-managed-disk"></a>Egy kezelt lemez csatolása

Felügyelt lemezek használata lehetővé teszi a virtuális gépek és a lemezek toofocus anélkül, hogy Azure Storage-fiókokat. Gyorsan hozhat létre, és felügyelt lemezt csatlakoztatni tooa VM használatával hello azonos Azure-erőforráscsoportot, vagy létrehozhat bármely lemezek számát, majd csatolja.


### <a name="attach-a-new-disk-tooa-vm"></a>Új lemez tooa VM csatolása

Ha csak egy új lemezt a virtuális gépen, használhatja a hello `az vm disk attach` parancsot.

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

### <a name="attach-an-existing-disk"></a>Meglévő lemez csatlakoztatása 

Sok esetben már létrehozott csatolni. Először a hello lemezazonosítót található, és akkor továbbítja az adott toohello `az vm disk attach` parancsot. hello alábbi példa egy lemezt használ létre `az disk create -g myResourceGroup -n myDataDisk --size-gb 50`.

```azurecli
# find hello disk id
diskId=$(az disk show -g myResourceGroup -n myDataDisk --query 'id' -o tsv)
az vm disk attach -g myResourceGroup --vm-name myVM --disk $diskId
```

hello kimeneti alábbihoz hasonló hello (használhatja hello `-o table` tooany tooformat hello parancskimenet a beállítás):

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


## <a name="attach-an-unmanaged-disk"></a>Nem kezelt lemez csatolása

Új lemez csatolása kész, ha nincs ellenére lemez létrehozásához a hello ugyanazt a tárfiókot, mint a virtuális Gépet. Típus `azure vm disk attach-new` toocreate és egy új GB lemezt csatolni a virtuális gép számára. Ha explicit módon nem talál egy tárfiókot, a lemez kerül hello azonos az operációsrendszer-lemez tartalmazó tárfiókot. Példa rendeli következő hello egy `50`GB szabad toohello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```

## <a name="connect-toohello-linux-vm-toomount-hello-new-disk"></a>Csatlakozás toohello Linux virtuális gép toomount hello új lemez
> [!NOTE]
> Ez a témakör tooa Virtuálisgép kapcsolódik felhasználónevek és jelszavak használatával. a virtuális Gépet, a toouse nyilvános és titkos kulcspárokat toocommunicate lásd [hogyan tooUse Azure Linux SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
> 
> 

TooSSH van szüksége az Azure virtuális gép toopartition formázása, és csatlakoztassa az új lemezt, a Linux virtuális gép használhassa azt. Ha még nem ismeri a Kapcsolódás a **ssh**, hello parancs felsorolásszerűen hello űrlap `ssh <username>@<FQDNofAzureVM> -p <hello ssh port>`, és a következőhöz hasonló hello:

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com -p 22
```

Kimenet

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

Most, hogy csatlakozott tooyour VM, készen áll a lemez tooattach.  Először megkeresi a hello lemez használatával `dmesg | grep SCSI` (hello módszert használja az új lemez eltérhetnek toodiscover). Ebben az esetben az alábbihoz hasonló:

```bash
dmesg | grep SCSI
```

Kimenet

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

Ez a témakör hello esetben hello és `sdc` lemez hello, amelyet azt szeretnénk, ha. Partíció rendelkező hello lemez `sudo fdisk /dev/sdc` – feltételezve, hogy az eset hello lemez volt `sdc`, és legyen egy elsődleges szabad a %1 partíció, és fogadja el hello többi alapértelmezett értéket.

```bash
sudo fdisk /dev/sdc
```

Kimenet

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

A partíció hello beírásával `p` hello parancssorba:

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

Írási és olvasási egy fájl rendszerpartíciót toohello hello segítségével **mkfs** parancs, a fájlrendszer típusát és hello név megadásával. Ebben a témakörben használunk `ext4` és `/dev/sdc1` a fent:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Kimenet

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

Most azt létre directory toomount hello fájl rendszer `mkdir`:

```bash
sudo mkdir /datadrive
```

Hello könyvtár használatával csatlakoztatja és `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

hello adatlemeze most már készen áll a toouse, `/datadrive`.

```bash
ls
```

Kimenet

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

tooensure hello meghajtót csatlakoztatni automatikusan, kell legyen újraindítás toohello /etc/fstab fájl hozzáadása után. Emellett javasoljuk, hogy hello UUID (univerzálisan egyedi azonosítója) használatos etc/fstab toorefer toohello meghajtó helyett csak hello eszköz neve (például `/dev/sdc1`). Hello OS lemezhiba rendszerindítás során észleli, ha hello UUID használatával elkerülhető hello helytelen lemezkonfigurációt alatt csatlakoztatott tooa megadott helyre. Adatlemezek fennmaradó volna lehet hozzárendelni ezeket azonos eszközazonosítókat. toofind hello UUID hello új meghajtó, használja a hello **blkid** segédprogram:

```bash
sudo -i blkid
```

hello kimeneti hasonló toohello következő néz ki:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

> [!NOTE]
> Nem megfelelően szerkesztése hello **/etc/fstab** fájl meghiúsulását eredményezheti. Ha nem ismeri, tekintse meg a toohello terjesztési dokumentációját olvashat, hogyan tooproperly szerkesztheti a fájlt. Ajánlott továbbá, hogy létrejött-e a fájl biztonsági másolata hello /etc/fstab szerkesztése előtt.
> 
> 

Ezután nyissa meg a hello **/etc/fstab** fájlt egy szövegszerkesztőben:

```bash
sudo vi /etc/fstab
```

A jelen példában használjuk hello UUID hello az új **/dev/sdc1** hello előző lépést, és hello csatlakozásipont létrehozott eszköz **/datadrive**. Adja hozzá a következő sor toohello vége hello hello **/etc/fstab** fájlt:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

> [!NOTE]
> Később eltávolítása adatlemezt fstab szerkesztése nélkül támadó hello VM toofail tooboot. A legtöbb azokat a terjesztéseket, adja meg vagy hello `nofail` és/vagy `nobootwait` fstab beállítások. Ezek a beállítások engedélyezése a rendszer tooboot, még akkor is, ha hello lemezhiba toomount rendszerindítás. A terjesztési dokumentációjában talál további információt ezekről a paraméterekről.
> 
> Hello **nofail** lehetőség biztosítja, hogy virtuális gép elindul, még akkor is, ha hello fájlrendszer sérült, vagy hello lemez nem létezik a rendszerindítás hello. Ez a beállítás nélkül felmerülhet viselkedés leírtak [nem SSH tooLinux VM tooFSTAB hibák miatt](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)

### <a name="trimunmap-support-for-linux-in-azure"></a>Az Azure Linux vágást/UNMAP támogatása
Egyes Linux kernelek vágást/UNMAP műveletek toodiscard támogatja a nem használt blokkok hello lemezen. Ez elsősorban fontos a standard szintű tárolást tooinform Azure törölt lapok már nem érvényes, és is elvesznek. Ez költség mentheti, ha nagy fájlok létrehozása, majd törli őket.

Két módon tooenable vágást támogatja a Linux virtuális Gépet. A szokásos módon olvassa el a terjesztési hello ajánlott megközelítést alkalmazva:

* Használjon hello `discard` lehetőség a csatlakoztatási `/etc/fstab`, például:

    ```bash
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```
* Az egyes esetekben hello `discard` lehetőség is van a teljesítményre. Alternatív megoldásként futtathatja a hello `fstrim` manuálisan parancsot a parancssorból hello, vagy felveheti Ön is tooyour crontab toorun rendszeresen:
  
    **Ubuntu**
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    **RHEL vagy CentOS**

    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a>Hibaelhárítás
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Következő lépések
* Ne feledje, hogy az új lemez nincs elérhető toohello VM Ha csak lehet írni, hogy információkat tooyour újraindítja [fstab](http://en.wikipedia.org/wiki/Fstab) fájlt.
* a Linux virtuális gép tooensure konfigurációja helytelen, tekintse át hello [a Linux-gépek teljesítményének optimalizálásához](optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) javaslatok.
* Bontsa ki a tárolási kapacitás további lemezek felvételével és [RAID konfigurálása](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további teljesítmény.

