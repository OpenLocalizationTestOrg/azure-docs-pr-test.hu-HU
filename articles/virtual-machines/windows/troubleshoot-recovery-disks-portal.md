---
title: "a Windows hello Azure-portálon a virtuális gép hibaelhárítási aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot Windows virtuális gépekkel kapcsolatos problémákat az Azure-ban használatával csatlakozó hello OS lemez tooa helyreállítási virtuális gép hello Azure-portálon"
services: virtual-machines-windows
documentationCenter: 
authors: genlin
manager: timlt
editor: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 396f70338fa39f80bb9adcb9244d3c83f2a233eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-windows-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-portal"></a>A Windows virtuális gépek hibaelhárításáról hello OS lemez tooa helyreállítási virtuális gép csatlakoztatása az Azure portál használatával hello
Ha a Windows rendszerű virtuális gép (VM) az Azure-ban rendszerindító vagy a lemez hibát tapasztal, szükség lehet a tooperform hibaelhárítási hello virtuális merevlemezen magát. Ilyenek például a sikertelen frissítés, amely megakadályozza hello méretű képes tooboot sikeresen lenne. Ez a cikk részletek hogyan toouse hello Azure portál tooconnect a virtuális merevlemez lemez tooanother Windows virtuális gép toofix ki a hibákat, majd hozza létre újból az eredeti virtuális gép.

## <a name="recovery-process-overview"></a>Helyreállítási folyamat áttekintése
hibaelhárítási folyamatának hello a következőképpen történik:

1. Törölje a hello VM észlelt problémák, hello virtuális merevlemezek tartása.
2. Csatolja, és a csatlakoztatási hello virtuális merevlemez tooanother Windows virtuális gép hibaelhárítási célból.
3. Csatlakoztassa a VM hibaelhárítási toohello. Szerkesztheti a fájlokat, vagy futtassa az olyan eszközöket toofix problémák hello eredeti virtuális merevlemez.
4. Válassza le a lemezképet, és válassza le hello virtuális merevlemezt a virtuális gép hibakeresési hello.
5. Hello eredeti virtuális merevlemez virtuális gép létrehozása.


## <a name="determine-boot-issues"></a>Határozza meg a rendszerindítási problémák
Miért a virtuális gép nem tud tooboot megfelelően, toodetermine hello rendszerindítási diagnosztika VM képernyőkép vizsgálja meg. Ilyenek például a sikertelen frissítés, vagy egy alapul szolgáló virtuális merevlemezek áthelyezése vagy törölhetők lenne.

Válassza ki a virtuális gép hello portálon, és görgessen lefelé toohello **támogatási + hibaelhárítás** szakasz. Kattintson a **rendszerindítási diagnosztika** tooview hello képernyőkép. Megjegyzés: a vonatkozó hibaüzeneteket vagy hibakódok toohelp határozza meg, miért hello VM kapcsolatban felmerült problémát. hello alábbi példa bemutatja a virtuális gépek vár a szolgáltatások leállítása:

![Megtekintés a virtuális gép rendszerindítási diagnosztika konzol naplók](./media/troubleshoot-recovery-disks-portal/screenshot-error.png)

Is **képernyőkép** toodownload egy hello VM képernyőkép rögzítése.


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

1. Nyissa meg a távoli asztali kapcsolat tooyour virtuális gép. Válassza ki a virtuális gép hello portálon, és kattintson a **Connect**. Töltse le és nyissa meg a hello RDP-kapcsolat fájlt. Adja meg a hitelesítő adatok toolog a virtuális gép tooyour a következők szerint:

    ![Jelentkezzen be a virtuális gép használata a távoli asztal tooyour](./media/troubleshoot-recovery-disks-portal/open-remote-desktop.png)

2. Nyissa meg **Kiszolgálókezelő**, majd jelölje be **fájl- és tárolási szolgáltatások**. 

    ![Válassza ki a fájl- és tárolási szolgáltatások Kiszolgálókezelőben](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

3. hello adatlemez automatikusan észlelt és csatolva. hello listája toosee csatlakozón kapcsolódó lemezek, válassza ki **lemezek**. Kiválaszthatja, hogy az adatok tooview lemezkötetekkel kapcsolatos információkat, beleértve a hello meghajtóbetűjelet. hello a következő példa azt mutatja be hello adatlemezt csatolni, és segítségével **F:**:

    ![Csatlakoztatott lemez és kötet információkat a Kiszolgálókezelőben](./media/troubleshoot-recovery-disks-portal/server-manager-disk-attached.png)


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Az eredeti virtuális merevlemez kapcsolatos problémák megoldása
Hello meglévő virtuális merevlemezzel csatlakoztatva is képes lemezvizsgálatok elvégzésére bármely karbantartási és hibaelhárítási lépéseket, igény szerint. Egyszer foglalkoztak hello problémák, folytassa a következő lépéseket hello.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Válassza le a lemezképet, és válassza le az eredeti virtuális merevlemez
Ha a hibák feloldása, válassza le hello meglévő virtuális merevlemezt a hibaelhárítási virtuális gépről. Nem használhat a virtuális merevlemez más virtuális gép hello virtuális merevlemez toohello hibaelhárítás a virtuális gép csatlakoztatása hello bérleti kiadásáig.

1. Hello RDP munkamenet tooyour VM, nyissa meg a **Kiszolgálókezelő**, majd jelölje be **fájl- és tárolási szolgáltatások**:

    ![Válassza a Kiszolgálókezelő fájl- és tárolási szolgáltatások](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

2. Válassza ki **lemezek** , és válassza ki az adatlemezt. Kattintson a jobb gombbal a adatok lemezen, és válassza ki **kapcsolat nélküli üzemmódra állítás**:

    ![Hello adatlemez állítja be a Kiszolgálókezelő offline állapotú](./media/troubleshoot-recovery-disks-portal/server-manager-set-disk-offline.png)

3. Most válassza le a hello hello virtuális gép a virtuális merevlemezt. Válassza ki a virtuális gép hello Azure-portálon **lemezek**. Válasszon a meglévő virtuális merevlemezt, majd kattintson **leválasztási**:

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
Ha a Kapcsolódás a virtuális gép tooyour problémát tapasztal, tekintse meg [hibaelhárítása RDP-kapcsolatok tooan Azure virtuális gép](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). A virtuális gépen futó alkalmazások elérésével problémákkal kapcsolatban lásd: [alkalmazás csatlakozási problémák a Windows virtuális gép](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Erőforrás-kezelő használatával kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).