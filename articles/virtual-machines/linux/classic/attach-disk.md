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
# <a name="how-tooattach-a-data-disk-tooa-linux-virtual-machine"></a>Hogyan tooAttach egy adatlemez tooa Linux virtuális gép
> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Lásd: hogyan túl[hello erőforrás-kezelő telepítési modellel adatlemezzel](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Csatolhat is üres és a lemezek, amelyek tartalmazzák az adatok tooyour Azure virtuális gépeken. Mindkét típusú lemezek a .vhd fájlok találhatók az Azure storage-fiók. Adja hozzá a lemezt tooa Linux gépi, hello lemez csatlakoztatása után meg kell tooinitialize és formázza azt, hogy készen álljon a használatra. Ez a cikk adatokat is üres és a lemezek már tartalmazó adatok tooyour virtuális gépeket, valamint, hogy toothen inicializálása és formázása új lemez csatolása.

> [!NOTE]
> A bevált gyakorlat toouse egy vagy több különálló lemezek toostore egy virtuális gép adatait. Egy Azure virtuális gép létrehozásakor rendelkezik az operációs rendszer és egy ideiglenes lemezzel. **Ne használjon hello ideiglenes toostore állandó lemezadatokat.** Hello név azt jelenti, mert csak az átmeneti tárolási biztosít. Kínál nincs redundancia vagy a biztonsági mentést, mert az nem található az Azure storage.
> hello ideiglenes lemez általában hello Azure Linux ügynök által felügyelt, és automatikusan csatlakoztatott túl**/mnt és az erőforrások** (vagy **/mnt** Ubuntu képek). A hello ugyanakkor, adatlemezt neve lehet hello Linux kernel hasonlót `/dev/sdc`, és toopartition, kell formázni, és a csatlakoztatási ehhez az erőforráshoz. Lásd: hello [Azure Linux ügynök felhasználói útmutató] [ Agent] részleteiről.
> 
> 

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>A Linux új adatlemezt inicializálása
1. SSH tooyour virtuális gép. További információkért lásd: [hogyan toolog a Linux rendszerű virtuális gép tooa][Logon].
2. Toofind hello eszközazonosító ezután a hello adatok lemez tooinitialize kell. Két módon toodo van, amely:
   
    a) Grep a SCSI-eszközökhöz a hello jelentkezik, például a hello a következő parancsot:
   
    ```bash
    sudo grep SCSI /var/log/messages
    ```
   
    A legutóbbi Ubuntu azokat a terjesztéseket, szükség lehet a toouse `sudo grep SCSI /var/log/syslog` , mert túl naplózás`/var/log/messages` alapértelmezés szerint le lehetnek tiltva.
   
    Hello utolsó adatlemez, amely hozzá lett adva a megjelenített köszönőüzenetei hello azonosítója található.
   
    ![Lemez köszönőüzenetei beolvasása](./media/attach-disk/scsidisklog.png)
   
    VAGY
   
    b) használata hello `lsscsi` parancs toofind hello eszközazonosító ki. `lsscsi` telepítheti vagy `yum install lsscsi` (Red Hat a disztribúció alapján) vagy `apt-get install lsscsi` (a Debian disztribúció alapján). Hello lemez által keresett megtalálhatja a *lun* vagy **logikaiegység-szám**. Például hello *lun* a csatolt hello lemezeket is könnyen látható `azure vm disk list <virtual-machine>` mint:

    ```azurecli
    azure vm disk list myVM
    ```

    hello hasonló toohello következő kimenete:

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
   
    Hasonlítsa össze ezeket az adatokat a hello kimenete `lsscsi` hello az azonos virtuális gép mintában:
   
    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```
   
    hello utolsó hello rekordot sorokban értéke hello *lun*. Lásd: `man lsscsi` további információt.
3. Hello parancssorba írja be a következő parancs toocreate hello eszközét:
   
    ```bash
    sudo fdisk /dev/sdc
    ```

4. Amikor a rendszer kéri, írja be a  **n**  toocreate partíció.

    ![Eszköz létrehozása](./media/attach-disk/fdisknewpartition.png)

5. Amikor a rendszer kéri, írja be a **p** toomake hello partíció hello elsődleges partíció. Típus **1** toomake hello első partíció, és írja be hello palack tooaccept hello alapértelmezett értéket adja meg. Egyes rendszerek képes hello alapértelmezett értékeinek hello megjelenítése első és utolsó szektorok, hello palack hello. Választhat tooaccept ezeket az alapértelmezett értékeket.

    ![A partíció](./media/attach-disk/fdisknewpartdetails.png)


6. Típus **p** hello lemez, amely alatt a toosee hello részleteit.

    ![Lista fizikai lemezeinek adatai](./media/attach-disk/fdiskpartitiondetails.png)


7. Típus **w** hello lemezek toowrite hello beállításai.

    ![Hello lemezen változások írása](./media/attach-disk/fdiskwritedisk.png)

8. Mostantól létrehozhat hello fájlrendszer hello új partíciót. Hello számú toohello eszköz Partícióazonosító hozzáfűzése (a példában a következő hello `/dev/sdc1`). hello alábbi példa partíciót hoz létre ext4 /dev/sdc1 meg:
   
    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
   
    ![-Fájl létrehozása](./media/attach-disk/mkfsext4.png)
   
   > [!NOTE]
   > SuSE Linux Enterprise 11 rendszerek csak ext4 fájlrendszerekhez támogatja a csak olvasási hozzáféréssel. Ezek a rendszerek tooformat hello új fájlrendszer ext4, hanem ext3 ajánlott.

9. Győződjön meg a könyvtár toomount hello új fájlrendszer, az alábbiak szerint:
   
    ```bash
    sudo mkdir /datadrive
    ```

10. Végül lehet hello meghajtót csatlakoztatni, az alábbiak szerint:
   
    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```
   
    hello adatlemeze most már készen áll a toouse, **/datadrive**.
   
    ![Hello directory és a csatlakoztatási hello lemez létrehozása](./media/attach-disk/mkdirandmount.png)

11. Hello új meghajtó túl/etc/fstab hozzáadása:
   
    tooensure hello meghajtót csatlakoztatni automatikusan, kell legyen újraindítás toohello /etc/fstab fájl hozzáadása után. Emellett javasoljuk, hogy hello UUID (univerzálisan egyedi azonosítója) /etc/fstab toorefer toohello helyett csak hello eszköz nevét (pl. /dev/sdc1) szerepel. Hello segítségével UUID elkerülhető hello helytelen lemezkonfigurációt megadott helyre, ha az operációs rendszer hello lemezhiba észleli a rendszerindítás során, és minden fennmaradó adatlemezt, majd alatt hozzárendelt azokat eszközazonosítókat csatlakoztatott tooa alatt. toofind hello UUID hello új meghajtó, használhatja a hello **blkid** segédprogram:
   
    ```bash
    sudo -i blkid
    ```
   
    hello kimenete a következő példa hasonló toohello néz ki:
   
    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

    > [!NOTE]
    > Nem megfelelően szerkesztése hello **/etc/fstab** fájl meghiúsulását eredményezheti. Ha nem ismeri, tekintse meg a toohello terjesztési dokumentációját olvashat, hogyan tooproperly szerkesztheti a fájlt. Ajánlott továbbá, hogy létrejött-e a fájl biztonsági másolata hello /etc/fstab szerkesztése előtt.

    Ezután nyissa meg a hello **/etc/fstab** fájlt egy szövegszerkesztőben:

    ```bash
    sudo vi /etc/fstab
    ```

    A jelen példában használjuk hello UUID hello az új **/dev/sdc1** hello előző lépést, és hello csatlakozásipont létrehozott eszköz **/datadrive**. Adja hozzá a következő sor toohello vége hello hello **/etc/fstab** fájlt:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
    ```

    Vagy SuSE Linux-alapú rendszereken szükség lehet a toouse némileg eltérő formátuma:

    ```sh
    /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    ```

    > [!NOTE]
    > Hello `nofail` lehetőség biztosítja, hogy virtuális gép elindul, még akkor is, ha hello fájlrendszer sérült, vagy hello lemez nem létezik a rendszerindítás hello. Ez a beállítás nélkül felmerülhet viselkedés leírtak [nem SSH tooLinux VM tooFSTAB hibák miatt](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    Most tesztelheti, hogy csatlakoztatva van-e a hello fájlrendszer megfelelően leválasztás, és ezután a fájlrendszer hello tudják, azaz hello példa csatlakoztatási pontot használ `/datadrive` hello létrehozott korábbi lépéseket:

    ```bash
    sudo umount /datadrive
    sudo mount /datadrive
    ```

    Ha hello `mount` hoz létre hiba, ellenőrizze hello/etc/fstab fájl helytelen szintaxisú. Ha további meghajtók és partíciók jönnek létre, adja meg azokat az/etc/fstab külön-külön is.

    Hello meghajtó írható ellenőrizze a parancs segítségével:

    ```bash
    sudo chmod go+w /datadrive
    ```

    > [!NOTE]
    > Ezt követően eltávolítása adatlemezt fstab szerkesztése nélkül támadó hello VM toofail tooboot. Ha ez gyakran előfordul, a legtöbb terjesztéseket adja meg vagy hello `nofail` és/vagy `nobootwait` fstab beállítások, amelyek lehetővé teszik a rendszer tooboot, még akkor is, ha hello lemezhiba toomount rendszerindítás. A terjesztési dokumentációjában talál további információt ezekről a paraméterekről.

### <a name="trimunmap-support-for-linux-in-azure"></a>Az Azure Linux vágást/UNMAP támogatása
Egyes Linux kernelek vágást/UNMAP műveletek toodiscard támogatja a nem használt blokkok hello lemezen. Ezek a műveletek elsősorban hasznosak standard tárolási tooinform Azure törölt lapok már nem érvényes, és is elvesznek. Lapok elvetése költség mentheti, ha nagy fájlok létrehozása, majd törli őket.

Két módon tooenable vágást támogatja a Linux virtuális Gépet. A szokásos módon olvassa el a terjesztési hello ajánlott megközelítést alkalmazva:

* Használjon hello `discard` lehetőség a csatlakoztatási `/etc/fstab`, például:

    ```sh
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
[!INCLUDE [virtual-machines-linux-lunzero](../../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Következő lépések
További a Linux virtuális gép használata a következő cikkek hello:

* [Hogyan toolog tooa virtuális gépen futó Linux][Logon]
* [Hogyan toodetach egy lemezt a Linux virtuális gép](detach-disk.md)
* [Hello Azure CLI használata hello klasszikus telepítési modell](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [A Linux virtuális gép RAID konfigurálása az Azure-ban](../configure-raid.md)
* [A Linux virtuális gép az Azure-ban LVM konfigurálása](../configure-lvm.md)

<!--Link references-->
[Agent]:../agent-user-guide.md
[Logon]:../mac-create-ssh-keys.md
