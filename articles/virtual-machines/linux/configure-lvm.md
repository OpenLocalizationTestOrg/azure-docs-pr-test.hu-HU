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
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>A Linux virtuális gép az Azure-ban LVM konfigurálása
Ez a dokumentum bemutatja, hogyan lehet hogyan tooconfigure logikai kötet Manager (LVM) az Azure virtuális gépen. Bár ez megvalósítható tooconfigure lemezen LVM csatolt toohello virtuális gép, alapértelmezés szerint a legtöbb felhő lemezképek nem fognak LVM konfigurált hello operációsrendszer-lemezzel. Ez az ismétlődő kötet csoportok tooprevent problémákat, ha az operációsrendszer-lemez van legalább egyszer hello csatolt tooanother hello a virtuális gép ugyanahhoz felosztáshoz és a típus, azaz a helyreállítás során. Ezért ajánlott csak toouse LVM hello adatok lemezeken.

## <a name="linear-vs-striped-logical-volumes"></a>Csíkozott köteteken tárolni logikai és lineáris
LVM lehet használt toocombine egy egyetlen tárolókötethez való fizikai lemezek számát. Alapértelmezés szerint LVM általában létrehoz lineáris logikai kötetek, ami azt jelenti, hogy együtt össze van-e fűzve hello fizikai tároló. Ebben az esetben olvasási/írási műveletek általában csak küld tooa egy lemezt. Ezzel szemben is létrehozhatunk olyan logikai csíkozott olvasási és írási esetén elosztott toomultiple lemezek hello kötet csoportból (azaz hasonló tooRAID0). Valószínűleg teljesítmény érdekében érdemes toostripe a logikai köteteket, hogy az olvasások és írások használják a mellékelt adatok lemezek.

Ez a dokumentum ismerteti, hogyan toocombine több adat lemezek kötetéről csoportba, és majd a csíkozott logikai kötet létrehozása. hello az alábbi lépésekre némileg általánosított toowork a legtöbb terjesztéseket. A legtöbb esetben hello segédprogramok és munkafolyamatok kezelése az Azure-on LVM nincsenek alapvetően más, mint más környezetekben. Szokásos módon is tekintse át a Linux forgalmazójával dokumentációk és ajánlott eljárások az adott terjesztési LVM használatával.

## <a name="attaching-data-disks"></a>Adatlemez csatlakoztatása
Egy általában érdemes toostart két vagy több üres adatlemezekkel rendelkező LVM használatakor. IO igényei szerint, kiválaszthatja a tooattach tárolódnak a standard szintű tárolást, a másolatot IO too500 ps / lemez vagy a prémium szintű storage IO too5000 ps lemezenként fel a lemezeket. Ez a cikk nem kerül részletes hogyan tooprovision, és csatlakoztassa az adatok lemezek tooa Linux virtuális gép. Tekintse meg a Microsoft Azure-cikk hello [lemezt csatlakoztatni](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) részletes útmutató hogyan tooattach egy üres adatok lemezre tooa Linux virtuális gépet az Azure.

## <a name="install-hello-lvm-utilities"></a>Hello LVM segédeszközök telepítése
* **Ubuntu**

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* **RHEL, CentOS & Oracle Linux**

    ```bash   
    sudo yum install lvm2
    ```

* **SLES 12 és openSUSE**

    ```bash   
    sudo zypper install lvm2
    ```

* **SLES 11**

    ```bash   
    sudo zypper install lvm2
    ```

    A SLES11 is szerkeszteni kell `/etc/sysconfig/lvm` és `LVM_ACTIVATED_ON_DISCOVERED` túl "Engedélyezés":

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a>LVM konfigurálása
A jelen útmutató indulunk ki csatolt adatok három lemezt, amely kifejezés tooas `/dev/sdc`, `/dev/sdd` és `/dev/sde`. Vegye figyelembe, hogy ezeket nem lehet a virtuális gép ugyanazon útvonalneveket hello legyen. Futtathatja a(z)`sudo fdisk -l`"vagy hasonló parancs toolist a rendelkezésre álló lemezek.

1. Készítse elő a hello fizikai kötetekre:

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. Hozzon létre egy kötet csoportot. Ebben a példában hívjuk hello kötet csoport `data-vg01`:

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. Hello logikai kötetek létrehozása. hello azt az alábbi parancs létrehoz nevű egyetlen logikai kötet `data-lv01` toospan hello teljes kötet csoport, de ne feledje, hogy azt is megvalósítható toocreate több logikai kötet hello kötet csoportban.

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. Kötet formázásának hello logikai

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > SLES11 használatával `-t ext3` ext4 helyett. SLES11 csak olvasási hozzáféréssel tooext4 fájlrendszerek támogatja.

## <a name="add-hello-new-file-system-tooetcfstab"></a>Hello új fájl rendszer túl/etc/fstab hozzáadása
> [!IMPORTANT]
> Nem megfelelően szerkesztése hello `/etc/fstab` fájl meghiúsulását eredményezheti. Ha nem ismeri, tekintse meg toohello terjesztési dokumentációjában olvashat, hogyan tooproperly szerkesztheti a fájlt. Is ajánlott, amelyek a biztonsági másolatának hello `/etc/fstab` fájl szerkesztése előtt jön létre.

1. Hozzon létre szükséges hello csatlakoztatási pontot az új fájlrendszer, például:

    ```bash  
    sudo mkdir /data
    ```

2. Keresse meg a hello logikai kötet elérési útja

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. Nyissa meg `/etc/fstab` egy szövegszerkesztőben, és adja hozzá egy bejegyzést a hello új fájlrendszer, például:

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    Ezt követően mentse és zárja be `/etc/fstab`.

4. Tesztelje, hogy hello `/etc/fstab` bejegyzés helyességét:

    ```bash    
    sudo mount -a
    ```

    Ha ezt a parancsot egy hibaüzenet ellenőrizze hello hello szintaxist `/etc/fstab` fájlt.
   
    Ezután futtassa az hello `mount` parancs tooensure hello fájlrendszer van csatlakoztatva:

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. (Választható) A FailSafe rendszerindító paraméterek`/etc/fstab`
   
    Terjesztések tartalmaz vagy hello `nobootwait` vagy `nofail` toohello hozzáadott paraméterek csatlakoztatási `/etc/fstab` fájlt. Ezek a paraméterek hibák engedélyezése, ha egy adott fájlrendszer csatlakoztatása és hello Linux rendszer toocontinue tooboot engedélyezi, akkor is, ha nem tooproperly csatlakoztatási hello RAID fájlrendszer. További információ ezekről a paraméterekről tooyour terjesztési dokumentációjában tájékozódhat.
   
    (Ubuntu). példa:

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a>TRIM/UNMAP támogatása
Egyes Linux kernelek vágást/UNMAP műveletek toodiscard támogatja a nem használt blokkok hello lemezen. Ezek a műveletek elsősorban hasznosak standard tárolási tooinform Azure törölt lapok már nem érvényes, és is elvesznek. Lapok elvetése költség mentheti, ha nagy fájlok létrehozása, majd törli őket.

Két módon tooenable vágást támogatja a Linux virtuális Gépet. A szokásos módon olvassa el a terjesztési hello ajánlott megközelítést alkalmazva:

- Használjon hello `discard` lehetőség a csatlakoztatási `/etc/fstab`, például:

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- Az egyes esetekben hello `discard` lehetőség is van a teljesítményre. Alternatív megoldásként futtathatja a hello `fstrim` manuálisan parancsot a parancssorból hello, vagy felveheti Ön is tooyour crontab toorun rendszeresen:

    **Ubuntu**

    ```bash 
    # sudo apt-get install util-linux
    # sudo fstrim /datadrive
    ```

    **RHEL vagy CentOS**

    ```bash 
    # sudo yum install util-linux
    # sudo fstrim /datadrive
    ```
