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
# <a name="configure-software-raid-on-linux"></a>Szoftveres RAID konfigurálása Linuxban
Egy általános forgatókönyv-toouse szoftveres RAID Linux virtuális gépek Azure toopresent több csatolt adatok egyetlen RAID eszközként lemezterület. Ez általában használt tooimprove teljesítmény kell és átviteli képest toousing csak egyetlen lemezt a továbbfejlesztett engedélyezése.

## <a name="attaching-data-disks"></a>Adatlemez csatlakoztatása
Két vagy több üres adatlemezek szükséges tooconfigure RAID eszköz.  hello elsődleges RAID eszköz létrehozása oka, hogy a lemez IO tooimprove teljesítményét.  IO igényei szerint, kiválaszthatja a tooattach tárolódnak a standard szintű tárolást, a másolatot IO too500 ps / lemez vagy a prémium szintű storage IO too5000 ps lemezenként fel a lemezeket. Ez a cikk nem halad részletes hogyan tooprovision, és csatlakoztassa az adatok lemezek tooa Linux virtuális gép.  Lásd: hello Microsoft Azure cikk [lemezt csatlakoztatni](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) hogyan tooattach egy üres adatok lemezre tooa Linux virtuális gépet az Azure részletes útmutatást.

## <a name="install-hello-mdadm-utility"></a>Hello mdadm segédprogram
* **Ubuntu**
```bash
sudo apt-get update
sudo apt-get install mdadm
```

* **CentOS & Oracle Linux**
```bash
sudo yum install mdadm
```

* **SLES és openSUSE**
```bash  
zypper install mdadm
```

## <a name="create-hello-disk-partitions"></a>Hozzon létre hello lemezpartíció
Ebben a példában /dev/sdc egyetlen lemezt a partíciót létrehozni azt. hello új lemezpartíciót /dev/sdc1 neve.

1. Start `fdisk` toobegin partíciók létrehozása

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

2. Nyomja meg az "n" hello Rákérdezés toocreate, egy  **n** új partíció:

    ```bash
    Command (m for help): n
    ```

3. Nyomja meg az "p" toocreate egy **p**lsődleges partíció:

    ```bash 
    Command action
            e   extended
            p   primary partition (1-4)
    ```

4. Nyomja meg az "1" tooselect partíciószám 1:

    ```bash
    Partition number (1-4): 1
    ```

5. Válassza ki a kiindulási pont hello új partíciót, vagy nyomja le az hello `<enter>` tooaccept hello alapértelmezett tooplace hello partíció hello elején szabad lemezterület hello hello meghajtón:

    ```bash   
    First cylinder (1-1305, default 1):
    Using default value 1
    ```

6. Válassza ki a hello partíció, például "+10G" típusú toocreate egy 10 GB partíció hello méretét. Vagy nyomja le az ENTER `<enter>` hello teljes meghajtó egyetlen partícióra létrehozása:

    ```bash   
    Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
    Using default value 1305
    ```

7. Ezután módosítsa az Azonosítót hello és **t**hello partíció hello alapértelmezett típus azonosító "83" (Linux) tooID "fd" (Linux raid automatikus):

    ```bash  
    Command (m for help): t
    Selected partition 1
    Hex code (type L toolist codes): fd
    ```

8. Végezetül hello partíció tábla toohello meghajtó írása, és zárja be a fdisk:

    ```bash   
    Command (m for help): w
    hello partition table has been altered!
    ```

## <a name="create-hello-raid-array"></a>Hello RAID tömb létrehozása
1. a következő példa lesz "paritásos" (RAID 0. szint) három lemezeken található három különböző adatokhoz (sdc1, sdd1, sde1) hello.  A parancs új RAID eszköz futtatása után **/dev/md127** jön létre. Vegye figyelembe azt is, hogy ha ezeket az adatokat lemezek azt korábban egy másik inaktív RAID tömb része lehet szükséges tooadd hello `--force` paraméter toohello `mdadm` parancs:

    ```bash  
    sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
        /dev/sdc1 /dev/sdd1 /dev/sde1
    ```

2. Hello fájlrendszer hello új RAID-eszköz létrehozása
   
    a. **CentOS, Oracle Linux, SLES 12, openSUSE és Ubuntu**

    ```bash   
    sudo mkfs -t ext4 /dev/md127
    ```
   
    b. **SLES 11**

    ```bash
    sudo mkfs -t ext3 /dev/md127
    ```
   
    c. **SLES 11** - boot.md engedélyezése és mdadm.conf létrehozása

    ```bash
    sudo -i chkconfig --add boot.md
    sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf
    ```
   
   > [!NOTE]
   > A SUSE rendszereken módosítások végrehajtását követően újraindításra lehet szükség. Ez a lépés *nem* SLES 12 szükséges.
   > 
   > 

## <a name="add-hello-new-file-system-tooetcfstab"></a>Hello új fájl rendszer túl/etc/fstab hozzáadása
> [!IMPORTANT]
> Helytelen a hello /etc/fstab fájl szerkesztése meghiúsulását eredményezheti. Ha nem ismeri, tekintse meg a toohello terjesztési dokumentációját olvashat, hogyan tooproperly szerkesztheti a fájlt. Ajánlott továbbá, hogy létrejött-e a fájl biztonsági másolata hello /etc/fstab szerkesztése előtt.

1. Hozzon létre szükséges hello csatlakoztatási pontot az új fájlrendszer, például:

    ```bash
    sudo mkdir /data
    ```
2. /Etc/fstab szerkesztésekor hello **UUID** használt tooreference hello fájl hello helyett a rendszer eszköz névnek kell lennie.  Használjon hello `blkid` segédprogram toodetermine hello UUID hello új fájlrendszer:

    ```bash   
    sudo /sbin/blkid
    ...........
    /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"
    ```

3. Nyissa meg a /etc/fstab egy szövegszerkesztőben, és adja hozzá egy bejegyzést a hello új fájlrendszer, például:

    ```bash   
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2
    ```
   
    Vagy a **SLES 11**:

    ```bash
    /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2
    ```
   
    Ezt követően mentse, és zárja be a /etc/fstab.

4. Tesztelje, hogy hello etc/fstab bejegyzés helyességét.

    ```bash  
    sudo mount -a
    ```

    Ha ezt a parancsot egy hibaüzenet, ellenőrizze a hello szintaxis hello /etc/fstab fájlban.
   
    Ezután futtassa az hello `mount` parancs tooensure hello fájlrendszer van csatlakoztatva:

    ```bash   
    mount
    .................
    /dev/md127 on /data type ext4 (rw)
    ```

5. (Választható) FailSafe rendszerindító paraméterek
   
    **fstab konfiguráció**
   
    Terjesztések tartalmaz vagy hello `nobootwait` vagy `nofail` toohello/etc/fstab fájl hozzáadott paraméterek csatlakoztatni. Ezek a paraméterek hibák engedélyezése, ha egy adott fájlrendszer csatlakoztatása és hello Linux rendszer toocontinue tooboot engedélyezi, akkor is, ha nem tooproperly csatlakoztatási hello RAID fájlrendszer. További információ ezekről a paraméterekről tekintse meg a tooyour terjesztési dokumentációját.
   
    (Ubuntu). példa:

    ```bash  
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2
    ```   

    **Linux rendszerindító paraméterek**
   
    A fenti paraméterek toohello hozzáadását, hello kernel paraméter "`bootdegraded=true`" hello rendszer tooboot lehetővé teheti, még akkor is, ha hello RAID kezeli, sérült vagy alacsonyabb, például ha egy adatmeghajtó véletlenül eltávolítják hello virtuális gépről. Alapértelmezés szerint ez is vezethet a nem rendszerindító rendszer.
   
    Tekintse meg az tooyour terjesztési dokumentációja hogyan tooproperly kernel paraméterek szerkesztése. Például terjesztések (CentOS, Oracle Linux SLES 11.) a következő paraméterek vehetők manuálisan toohello "`/boot/grub/menu.lst`" fájl.  Ubuntu ennek a paraméternek is hozzáadható toohello `GRUB_CMDLINE_LINUX_DEFAULT` változó a "/ etc/alapértelmezett/lárvajárat".


## <a name="trimunmap-support"></a>TRIM/UNMAP támogatása
Egyes Linux kernelek vágást/UNMAP műveletek toodiscard támogatja a nem használt blokkok hello lemezen. Ezek a műveletek elsősorban hasznosak standard tárolási tooinform Azure törölt lapok már nem érvényes, és is elvesznek. Lapok elvetése költség mentheti, ha nagy fájlok létrehozása, majd törli őket.

> [!NOTE]
> RAID nem lehetséges, hogy ki elvetési parancsok, ha hello adatrészlet méretének hello tömb tooless hello alapértelmezett (512KB) van beállítva. Ennek az az oka hello megfeleltetésének törlése a gazdagép hello granularitási egyben 512KB. Ha módosította-e hello tömb adatrészlet méretének keresztül mdadm tartozó `--chunk=` paramétert, majd a vágás/megfeleltetésének törlése kérelmek előfordulhat, hogy figyelmen kívül lesz hagyva hello kernel.

Két módon tooenable vágást támogatja a Linux virtuális Gépet. A szokásos módon olvassa el a terjesztési hello ajánlott megközelítést alkalmazva:

- Használjon hello `discard` lehetőség a csatlakoztatási `/etc/fstab`, például:

    ```bash
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,discard  0  2
    ```

- Az egyes esetekben hello `discard` lehetőség is van a teljesítményre. Alternatív megoldásként futtathatja a hello `fstrim` manuálisan parancsot a parancssorból hello, vagy felveheti Ön is tooyour crontab toorun rendszeresen:

    **Ubuntu**

    ```bash
    # sudo apt-get install util-linux
    # sudo fstrim /data
    ```

    **RHEL vagy CentOS**
    ```bash
    # sudo yum install util-linux
    # sudo fstrim /data
    ```
