---
title: "a Linux virtuális gép hibaelhárítás hello Azure-portálon a aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot Linux virtuális gépekkel kapcsolatos problémákat használatával csatlakozó hello OS lemez tooa helyreállítási virtuális gép hello Azure-portálon"
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/14/2016
ms.author: iainfou
ms.openlocfilehash: 794daa06d7436215af84a61ab9088524254c47df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-portal"></a>Linux virtuális gép hibaelhárításáról hello OS lemez tooa helyreállítási virtuális gép csatlakoztatása az Azure portál használatával hello
Ha a Linux virtuális gép (VM) rendszerindító vagy a lemez hibát tapasztal, szükség lehet a tooperform hibaelhárítási hello virtuális merevlemezen magát. Ilyenek például a bejegyzés érvénytelen lenne `/etc/fstab` , amely megakadályozza hello méretű képes tooboot sikeresen. Ez a cikk részletek hogyan toouse hello Azure portál tooconnect a virtuális merevlemez lemez tooanother Linux virtuális gép toofix ki a hibákat, majd hozza létre újból az eredeti virtuális gép.

## <a name="recovery-process-overview"></a>Helyreállítási folyamat áttekintése
hibaelhárítási folyamatának hello a következőképpen történik:

1. Törölje a hello VM észlelt problémák, hello virtuális merevlemezek tartása.
2. Csatolja, és a csatlakoztatási hello virtuális merevlemez tooanother Linux virtuális gép hibaelhárítási célból.
3. Csatlakoztassa a VM hibaelhárítási toohello. Szerkesztheti a fájlokat, vagy futtassa az olyan eszközöket toofix problémák hello eredeti virtuális merevlemez.
4. Válassza le a lemezképet, és válassza le hello virtuális merevlemezt a virtuális gép hibakeresési hello.
5. Hello eredeti virtuális merevlemez virtuális gép létrehozása.


## <a name="determine-boot-issues"></a>Határozza meg a rendszerindítási problémák
Ellenőrizze a hello rendszerindítási diagnosztika és a virtuális gép képernyőkép toodetermine miért a virtuális gép nem tud tooboot megfelelően. Ilyenek például a bejegyzés érvénytelen lenne `/etc/fstab`, vagy egy alapul szolgáló virtuális merevlemezek áthelyezése vagy törölhetők.

Válassza ki a virtuális gép hello portálon, és görgessen lefelé toohello **támogatási + hibaelhárítás** szakasz. Kattintson a **rendszerindítási diagnosztika** tooview konzol köszönőüzenetei folyamatos átviteli a virtuális gépről. Felülvizsgálati hello-konzol naplói toosee, ha azt is meghatározhatja, miért hello VM kapcsolatban felmerült problémát. hello alábbi példa bemutatja a virtuális gépek elakadt a karbantartási módba manuális beavatkozásra van szükség:

![Megtekintés a virtuális gép rendszerindítási diagnosztika konzol naplók](./media/troubleshoot-recovery-disks-portal/boot-diagnostics-error.png)

Is **képernyőkép** hello felső részén hello rendszerindítási diagnosztika napló toodownload egy hello VM képernyőkép rögzítése között.


## <a name="view-existing-virtual-hard-disk-details"></a>Meglévő virtuális merevlemez részleteinek megtekintése
A virtuális merevlemez tooanother VM csatolhat, meg kell tooidentify hello neve hello virtuális merevlemez (VHD). 

Válassza ki az erőforráscsoport hello portálról, majd válassza ki a tárfiók. Kattintson a **Blobok**, a példában a következő hello:

![Válassza ki a tárolási BLOB](./media/troubleshoot-recovery-disks-portal/storage-account-overview.png)

Általában akkor nevű tárolót **VHD-k** , amely a virtuális merevlemezeket tárolja. Válassza ki a hello tároló tooview virtuális merevlemezek listáját. Megjegyzés: hello nevet a virtuális merevlemez (hello előtag az általában a virtuális gép nevét hello):

![A tároló virtuális merevlemez azonosítása](./media/troubleshoot-recovery-disks-portal/storage-container.png)

Válassza ki a meglévő virtuális merevlemez hello listából, és másolja a hello URL-CÍMÉT használja a következő lépéseket hello:

![Meglévő virtuális merevlemez URL-Címének másolása](./media/troubleshoot-recovery-disks-portal/copy-vhd-url.png)


## <a name="delete-existing-vm"></a>Meglévő virtuális gép törlése
A virtuális merevlemezek és a virtuális gépek az Azure-erőforrások két különböző típusa. A virtuális merevlemez hello operációs rendszert illeti, alkalmazások és konfigurációk tárolására. hello virtuális gépért csak metaadatokat, amelyek hello vagy a hely határozza meg, és hivatkozik arra az erőforrások, például egy virtuális merevlemezt vagy virtuális hálózati kártya (NIC). Minden virtuális merevlemez rendelkezik-e létrehozásakor kell csatolni a címbérlet tooa virtuális gép. Bár adatlemezt csatolni, és leválasztott hello virtuális gép futása közben is, hello operációsrendszer-lemez nem választható le, kivéve, ha a virtuális gép erőforrásához hello törlődik. hello bérleti tooassociate hello OS lemezt a virtuális gépek továbbra is, akkor is, ha ezt a virtuális Gépet felszabadított és leállított állapotban van.

első lépés toorecover hello a virtuális gép toodelete hello VM erőforrás magát. Virtuális gép törlése hello hello virtuális merevlemezek elhagyja a tárfiókban lévő. Hello a virtuális gép törlődik, miután hello virtuális merevlemez tooanother VM tootroubleshoot csatolja, és hárítsa el a hello hibákat.

A virtuális gép hello portálon válassza **törlése**:

![Virtuális gép rendszerindítási diagnosztika képernyőfelvétel rendszerindítási hiba](./media/troubleshoot-recovery-disks-portal/stop-delete-vm.png)

Várjon, amíg hello virtuális gép törlése előtt hello virtuális merevlemez tooanother virtuális gép csatlakoztatása befejeződött. hello virtuális merevlemezt, amely társítja azt hello VM hello bérlete hello virtuális merevlemez tooanother VM előtt kiadott toobe kell.


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a>Meglévő virtuális merevlemez tooanother VM csatolása
A hello ezután néhány lépést, akkor egy másik virtuális gép hibaelhárítási célból. Hello VM toobe képes toobrowse hibaelhárítási meglévő virtuális merevlemez toothis csatolja, és hello lemez tartalmának szerkesztéséhez. Ez a folyamat toocorrect lehetővé teszi bármely konfigurációs hibák vagy a felülvizsgálati további alkalmazások vagy a rendszer naplófájljait, például. Válasszon, vagy hozzon létre egy másik virtuális gép toouse hibaelhárítási célból.

1. Válassza ki az erőforráscsoport hello portálról, majd válassza ki a hibaelhárítási virtuális Gépet. Válassza ki **lemezek** majd **Csatolás meglévő**:

    ![Hello portálon meglévő lemez csatolása](./media/troubleshoot-recovery-disks-portal/attach-existing-disk.png)

2. tooselect a meglévő virtuális merevlemez kattintson **VHD-fájl**:

    ![Meglévő VHD keresése](./media/troubleshoot-recovery-disks-portal/select-vhd-location.png)

3. A tárfiók és tároló, majd kattintson a meglévő virtuális Merevlemezt. Kattintson a hello **válasszon** tooconfirm gomb szabadon választott:

    ![Meglévő VHD kiválasztása](./media/troubleshoot-recovery-disks-portal/select-vhd.png)

4. A most kijelölt virtuális merevlemez, és kattintson **OK** tooattach hello meglévő virtuális merevlemez:

    ![Ellenőrizze a meglévő virtuális merevlemez csatlakoztatása](./media/troubleshoot-recovery-disks-portal/attach-disk-confirm.png)

5. Néhány másodperc múlva hello **lemezek** a virtuális gép ablaktábla listázza a meglévő virtuális merevlemez csatlakoztatva adatlemezt számára:

    ![Adatlemezként csatlakoztatott meglévő virtuális merevlemez](./media/troubleshoot-recovery-disks-portal/attached-disk.png)


## <a name="mount-hello-attached-data-disk"></a>Hello csatolt adatlemez csatlakoztatása

> [!NOTE]
> hello következő példák részletesen hello lépéseit egy Ubuntu virtuális gép. Red Hat Enterprise Linux és SUSE, például a különböző Linux distro használata hello naplófájl helye és `mount` parancsok kissé eltérő lehet. A parancsok hello megfelelő változásokat az adott distro a tekintse meg a toohello dokumentációját.

1. SSH tooyour hibaelhárítási hello megfelelő hitelesítő adatokat használó virtuális gépek. Ha a lemez hello első adatok csatlakoztatott lemez tooyour VM hibaelhárítási, valószínűleg csatlakoztatva van túl`/dev/sdc`. Használjon `dmseg` toolist csatlakoztatott lemezekkel:

    ```bash
    dmesg | grep SCSI
    ```
    hello hasonló toohello a következő példa a kimenetre:

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    A fenti példa hello, hello operációsrendszer-lemez jelenleg `/dev/sda` hello ideiglenes lemez megadott összes virtuális Géphez és `/dev/sdb`. Ha több adatlemezek, hogy legyen `/dev/sdd`, `/dev/sde`, és így tovább.

2. Hozzon létre egy könyvtárat toomount a meglévő virtuális merevlemez. hello alábbi példa létrehoz egy könyvtárat nevű `troubleshootingdisk`:

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Ha több partíciót a meglévő virtuális merevlemezen, csatlakoztassa a szükséges hello partíció. hello alábbi példa csatlakoztatja, elsődleges partíció első hello `/dev/sdc1`:

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > Ajánlott toomount adatlemezek virtuális gépeken Azure használatával hello univerzálisan egyedi azonosító (UUID) hello virtuális merevlemez. A jelen rövid hibaelhárítási esetben csatlakoztatását a hello virtuális merevlemez segítségével hello UUID nincs szükség. Azonban a normál használja, a Szerkesztés `/etc/fstab` toomount virtuális merevlemezek helyett eszköznév UUID okozhat hello VM toofail tooboot.


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Az eredeti virtuális merevlemez kapcsolatos problémák megoldása
Hello meglévő virtuális merevlemezzel csatlakoztatva is képes lemezvizsgálatok elvégzésére bármely karbantartási és hibaelhárítási lépéseket, igény szerint. Egyszer foglalkoztak hello problémák, folytassa a következő lépéseket hello.

## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Válassza le a lemezképet, és válassza le az eredeti virtuális merevlemez
Ha a hibák feloldása, válassza le hello meglévő virtuális merevlemezt a hibaelhárítási virtuális gépről. Nem használhat a virtuális merevlemez más virtuális gép hello virtuális merevlemez toohello hibaelhárítás a virtuális gép csatlakoztatása hello bérleti kiadásáig.

1. A hello a virtuális gép hibakeresési SSH munkamenet tooyour leválasztása hello létező virtuális merevlemezt. Először kívül hello szülőkönyvtárában a csatlakoztatási pont módosítása:

    ```bash
    cd /
    ```

    Most leválasztása hello létező virtuális merevlemezt. hello alábbi példa leválasztja hello eszközéből `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Most válassza le a hello hello virtuális gép a virtuális merevlemezt. Válassza ki a virtuális gép hello portálon, és kattintson a **lemezek**. Válasszon a meglévő virtuális merevlemezt, majd kattintson **leválasztási**:

    ![Válassza le a meglévő virtuális merevlemez](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    Várjon, amíg hello VM van az a folytatás előtt hello adatlemez leválasztása sikeresen megtörtént.

## <a name="create-vm-from-original-hard-disk"></a>Virtuális gép eredeti merevlemez létrehozása
toocreate egy virtuális Gépet az eredeti virtuális merevlemez használata [Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). hello sablon létrehozni meglévő virtuális hálózatban, hello virtuális merevlemez URL-címet a hello segítségével központilag telepíti egy virtuális gép korábbi parancsot. Kattintson a hello **tooAzure telepítése** gombra kattint, az alábbiak szerint:

![A sablont a Githubból a virtuális gép üzembe helyezése](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

hello sablon betöltődik a hello Azure-portál a központi telepítés. Írja be az új virtuális gép és a meglévő Azure-erőforrások hello nevét, és illessze be a hello URL-cím tooyour létező virtuális merevlemezt. toobegin hello központi telepítés, kattintson a **beszerzési**:

![Telepítse a virtuális Gépet sablonból](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a>Engedélyezze újra a rendszerindítási diagnosztika
Hello létező virtuális merevlemezt a virtuális Gépet hoz létre, ha rendszerindítási diagnosztika automatikusan nem lehet engedélyezni. toocheck hello rendszerindítási diagnosztika állapotát, és kapcsolja be, ha szükséges, válassza ki a virtuális gép hello portálon. A **figyelés**, kattintson a **diagnosztikai beállítások**. Győződjön meg arról, hello állapota **a**, és hello pipa mellett túl**rendszerindítási diagnosztika** van kiválasztva. Ha bármilyen módosításhoz kattintson **mentése**:

![Rendszerindítási diagnosztika beállításainak frissítése](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="next-steps"></a>Következő lépések
Ha a Kapcsolódás a virtuális gép tooyour problémát tapasztal, tekintse meg [hibaelhárítása SSH kapcsolatok tooan Azure virtuális gép](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). A virtuális gépen futó alkalmazások elérésével problémákkal kapcsolatban lásd: [alkalmazás csatlakozási problémák a Linux virtuális gép](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Erőforrás-kezelő használatával kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
