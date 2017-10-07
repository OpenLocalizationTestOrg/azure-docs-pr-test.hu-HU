---
title: "a helyi Windows-jelszó nélkül Azure ügynök aaaReset |} Microsoft Docs"
description: "Hogyan tooreset hello-e a helyi Windows felhasználói fiók jelszavát, amikor hello Azure Vendég ügynöke nincs telepítve vagy nem működik a virtuális gép"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: cf353dd3-89c9-47f6-a449-f874f0957013
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/07/2017
ms.author: iainfou
ms.openlocfilehash: c559c31ea142f9cf50d2c5b6182c5355fec9bac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-local-windows-password-for-azure-vm"></a>Hogyan tooreset helyi Windows-jelszó Azure virtuális Gépen
Visszaállíthatja a hello helyi Windows-jelszó a virtuális gépek az Azure-ban hello [Azure-portálon vagy az Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) megadott hello Azure Vendég-ügynök telepítve van. Ez a metódus hello elsődleges módon tooreset egy Azure virtuális gép jelszavát. Ha hibát tapasztal hello Azure Vendég ügynök nem válaszol, vagy egy egyéni lemezkép feltöltése után tooinstall sikertelen, alaphelyzetbe állíthatja a manuálisan egy Windows-jelszavát. Ez a cikk részletesen, hogyan tooreset egy helyi fiók jelszavát csatolásával hello forrás operációs rendszer virtuális lemez tooanother virtuális gép. 

> [!WARNING]
> Csak utolsó lehetőségként használja a ezt a folyamatot. Mindig próbálja tooreset hello segítségével jelszó [Azure-portálon vagy az Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) első.
> 
> 

## <a name="overview-of-hello-process"></a>Hello folyamat áttekintése
egy helyi jelszót alaphelyzetbe állítani a Windows virtuális gépek Azure-ban, amikor nincs hozzáférési toohello Azure Vendég ügynök hello core lépései a következőképpen történik:

* Hello forrás virtuális gép törlése. hello virtuális lemezek jelennek meg.
* Hello forrás virtuális gép operációs rendszer lemez tooanother VM hello csatolása ugyanazon a helyen belül az Azure-előfizetéshez. Ez a virtuális gép virtuális gép hibakeresési hivatkozott tooas hello.
* Néhány konfigurációs fájlt, a hello forrás virtuális gép operációsrendszer-lemez VM hibaelhárítási hello segítségével hozzon létre.
* Hello virtuális gép operációs rendszer lemezt leválasztani a virtuális gép hibakeresési hello.
* A Resource Manager sablon toocreate egy virtuális gépet a hello eredeti virtuális lemezt használja.
* Amikor hello új virtuális gép rendszerindításnál hello olyan konfigurációs fájlt hoz létre a frissítés hello szükséges hello felhasználó jelszavát.

## <a name="detailed-steps"></a>Részletes lépések
Mindig próbálja tooreset hello segítségével jelszó [Azure-portálon vagy az Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hello lépések előtt. Győződjön meg arról, hogy a virtuális gép biztonsági másolata megkezdése előtt. 

1. Törölje az érintett virtuális gép hello Azure-portálon. Virtuális gép törlése hello csak törli hello metaadatok, az Azure virtuális gép hello hello hivatkozását. virtuális lemezek hello megmaradnak hello virtuális gép törlésekor:
   
   * Kattintson a hello Azure-portálon válassza hello VM *törlése*:
     
     ![Meglévő virtuális gép törlése](./media/reset-local-password-without-agent/delete_vm.png)
2. Hello forrás virtuális gép operációs rendszer lemez toohello VM hibaelhárítási csatolni. hibaelhárítás a virtuális gép hello hello kell lennie és hello forrás virtuális gép operációsrendszer-lemez ugyanabban a régióban (például `West US`):
   
   * Válassza ki a virtuális gép hibaelhárítás hello Azure-portálon a hello. Kattintson a *lemezek* | *Csatolás meglévő*:
     
     ![Meglévő lemez csatolása](./media/reset-local-password-without-agent/disks_attach_existing.png)
     
     Válassza ki *VHD-fájl* , és válassza a hello storage-fiók, amely tartalmazza a virtuális gép – forrásként:
     
     ![Adattároló fiók kiválasztása](./media/reset-local-password-without-agent/disks_select_storageaccount.PNG)
     
     Hello forrás-tároló választása. hello forrás tároló van általában *VHD-k*:
     
     ![Válassza ki a tároló](./media/reset-local-password-without-agent/disks_select_container.png)
     
     Válassza ki a hello az operációs rendszer virtuális merevlemez tooattach. Kattintson a *válasszon* toocomplete hello folyamat:
     
     ![Válassza ki a forrás virtuális lemez](./media/reset-local-password-without-agent/disks_select_source_vhd.png)
3. Csatlakozzon a virtuális gép használata a távoli asztal hibaelhárítási toohello, és győződjön meg arról, hello forrás virtuális gép operációsrendszer-lemez látható:
   
   * Válassza ki a virtuális gép hibaelhárítás hello Azure-portálon a hello, és kattintson a *Connect*.
   * Nyissa meg a hello RDP-fájlt, amely letölti. Hello felhasználóneve és jelszava megadása a virtuális gép hibakeresési hello.
   * A Fájlkezelőben keresse meg a csatolt hello adatlemez. Ha hello a virtuális gép virtuális merevlemez forrás hello VM hibakeresési adatok kizárólag csatlakoztatott lemez toohello, meg kell hello F: meghajtó:
     
     ![Csatolt adatlemez megtekintése](./media/reset-local-password-without-agent/troubleshooting_vm_fileexplorer.png)
4. Hozzon létre `gpt.ini` a `\Windows\System32\GroupPolicy` hello forrás Virtuálisgép-meghajtón (ha létezik a gpt.ini, nevezze át toogpt.ini.bak):
   
   > [!WARNING]
   > Győződjön meg arról, hogy véletlenül nem hello következő C:\Windows a fájlok létrehozása, az operációs rendszer meghajtóját VM hibaelhárítási hello hello. Hozzon létre a következő fájlok hello operációsrendszer-meghajtón a forrás virtuális gép adatok lemezként csatolt hello.
   > 
   > 
   
   * Adja hozzá az alábbi be hello hello `gpt.ini` létrehozott fájlt:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![Gpt.ini létrehozása](./media/reset-local-password-without-agent/create_gpt_ini.png)
5. Hozzon létre `scripts.ini` a `\Windows\System32\GroupPolicy\Machine\Scripts`. Győződjön meg arról, rejtett mappákba jelennek meg. Szükség esetén hozzon létre hello `Machine` vagy `Scripts` mappák.
   
   * Adja hozzá a következő sorokat hello hello `scripts.ini` létrehozott fájlt:
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Scripts.ini létrehozása](./media/reset-local-password-without-agent/create_scripts_ini.png)
6. Hozzon létre `FixAzureVM.cmd` a `\Windows\System32` hello tartalma a következő, hogy a `<username>` és `<newpassword>` saját értékekkel:
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add

    ```

    ![FixAzureVM.cmd létrehozása](./media/reset-local-password-without-agent/create_fixazure_cmd.png)
   
    Új jelszó hello definiálásakor meg kell felelnie konfigurált hello összetettségi követelményeknek a virtuális gép számára.
7. Azure-portálon hello lemezt leválasztani a virtuális gép hibakeresési hello:
   
   * Jelölje ki azt a virtuális gép hibaelhárítás hello Azure-portálon a hello *lemezek*.
   * SELECT hello adatlemezt csatolni a 2. lépésben, kattintson a *leválasztási*:
     
     ![Leválasztja a lemezt](./media/reset-local-password-without-agent/detach_disk.png)
8. Mielőtt létrehozna egy virtuális Gépet, szerezzen be hello URI tooyour forrás operációsrendszer-lemez:
   
   * Jelölje be hello storage-fiókot hello Azure-portálon kattintson *Blobok*.
   * Hello-tároló választása. hello forrás tároló van általában *VHD-k*:
     
     ![Válassza ki a tárolási fiók blob](./media/reset-local-password-without-agent/select_storage_details.png)
     
     Válassza ki a forrás virtuális gép operációs rendszer virtuális Merevlemezt, majd kattintson a hello *másolási* gomb következő toohello *URL-cím* nevét:
     
     ![Lemezmásolás URI](./media/reset-local-password-without-agent/copy_source_vhd_uri.png)
9. A virtuális gépek hello forrás virtuális gép operációsrendszer-lemez létrehozása:
   
   * Használjon [Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) toocreate speciális virtuális Merevlemezt a virtuális gép. Kattintson a hello `Deploy tooAzure` gomb tooopen hello Azure-portálon kijelöli a hello sablonalapú adatokkal.
   * Ha tooretain összes hello előző beállítása hello virtuális gép, jelölje be *Szerkesztés sablon* tooprovide a meglévő virtuális hálózatot, alhálózatot, hálózati adapter vagy nyilvános IP-cím.
   * A hello `OSDISKVHDURI` paraméter szövegmezőben, Beillesztés hello lépést megelőző hello az beszerzése a forrás VHD URI:
     
     ![Virtuális gép létrehozása sablonból](./media/reset-local-password-without-agent/create_new_vm_from_template.png)
10. Után hello új virtuális gép fut, csatlakozzon a virtuális gép használata a távoli asztal hello hello megadott új jelszó toohello `FixAzureVM.cmd` parancsfájl.
11. A távoli munkamenet toohello az új virtuális Gépet, a következő eltávolítása hello fájlok tooclean hello környezetet:
    
    * A %windir%\System32
      * Távolítsa el a FixAzureVM.cmd
    * A %windir%\System32\GroupPolicy\Machine\
      * Távolítsa el a scripts.ini
    * A %windir%\System32\GroupPolicy
      * Távolítsa el a gpt.ini (Ha a gpt.ini létezett, és hogy átnevezte toogpt.ini.bak, nevezze át hello .bak fájl hátsó toogpt.ini)

## <a name="next-steps"></a>Következő lépések
Ha továbbra is tud kapcsolódni a távoli asztal használatával, lásd: hello [RDP hibaelhárítási útmutató](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Hello [részletes hibaelhárítási útmutatója RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ellenőrzi, hogy bizonyos lépésekre helyett módszerek hibaelhárítás. Emellett [az Azure támogatási kérést nyithat](https://azure.microsoft.com/support/options/) gyakorlati segítségért.

