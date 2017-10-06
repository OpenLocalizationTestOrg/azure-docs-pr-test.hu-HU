---
title: "aaaManage erőforrás-kezelő telepített virtuális gépek biztonsági mentéseinek |} Microsoft Docs"
description: "Megtudhatja, hogyan toomanage és a figyelő a Resource Manager telepített virtuális gépek biztonsági mentéseinek"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: f3050283-d60f-472d-b464-cb844e70d67e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: trinadhk;markgal
ms.openlocfilehash: 241fc4b2a29c5cd8b8b0ee8efbf8ba4e96c6a7ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-machine-backups"></a>Azure-beli virtuális gépek biztonsági másolatainak kezelése
> [!div class="op_single_selector"]
> * [Azure virtuális gép biztonsági mentések kezelése](backup-azure-manage-vms.md)
> * [Klasszikus virtuális gép biztonsági mentések kezelése](backup-azure-manage-vms-classic.md)
>
>

Ez a cikk útmutatást a virtuális gép biztonsági másolatok kezelésére, és hello biztonsági mentésekkel kapcsolatos riasztások adatait, amely a hello portál Irányítópultjára. Ez a cikk útmutatást hello toousing virtuális gépek Recovery Services-tárolók vonatkozik. Ez a cikk nem foglalkozik a virtuális gépek hello létrehozását, se nem ismertetik hogyan tooprotect virtuális gépek. A Recovery Services-tároló virtuális gépek Azure Resource Manager üzembe az Azure-ban ellátásáról ismertetése, lásd: [először: készítsen biztonsági mentést a Recovery Services-tároló virtuális gépek tooa](backup-azure-vms-first-look-arm.md).

## <a name="manage-vaults-and-protected-virtual-machines"></a>Tárolók és a védett virtuális gépek kezelése
Hello Azure-portálon hello Recovery Services-tároló irányítópult biztosít hozzáférést tooinformation kapcsolatos hello tároló többek között:

* hello legfrissebb biztonsági mentési pillanatképet, amely egyben hello legújabb visszaállítási pont < br\>
* a biztonsági mentési házirend hello < br\>
* teljes méret minden biztonsági mentési pillanatképek < br\>
* hello tárolóban védett virtuális gépek száma < br\>

Egy virtuális gép biztonsági másolatából számos felügyeleti feladatot a kezdő hello irányítópulton hello tároló megnyitása. Azonban mivel tárolók lehetnek több elemet (vagy több virtuális gép), nyissa meg a tooview egy adott virtuális gép vonatkozó részletek használt tooprotect hello tároló elem irányítópult. hello alábbi eljárás bemutatja, hogyan tooopen hello *tároló irányítópult* és folytassa a toohello *tároló elem irányítópult*. Nincsenek "tippek" mindkét eljárásnál, amelyek hogyan tooadd hello tároló és a tároló elem toohello Azure irányítópult hello PIN-kód toodashboard parancs használatával. PIN-kód toodashboard egy helyi toohello tárolóhoz vagy az elem létrehozásának módja. Általános jellegű parancsok a hello helyi is végrehajthat.

> [!TIP]
> Ha több irányítópultok és panel nyílik meg, csúszkával hello sötét-kék hello ablak tooslide hello Azure irányítópult oda-vissza hello alján.
>
>

![A csúszka teljes áttekintése](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-hello-dashboard"></a>Nyissa meg a Recovery Services-tároló hello irányítópulton:
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Hello központ menüben kattintson a **Tallózás** hello az erőforrások listájához, írja be a **Recovery Services**. Írja be megkezdése előtt, hello szűrők megjelenítése a megadott feltételeknek. Kattintson a **Recovery Services-tároló** elemre.

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-manage-vms/browse-to-rs-vaults.png) <br/>

    Recovery Services-tárolók listája hello jelennek meg.

    ![Helyreállítási szolgáltatások listája tárolók ](./media/backup-azure-manage-vms/list-o-vaults.png) <br/>

   > [!TIP]
   > Ha egy tároló toohello Azure irányítópult rögzíti, hogy a tároló érhető el azonnal hello Azure-portál megnyitásakor. toopin tároló toohello irányítópult hello tároló listában, kattintson a jobb gombbal a hello tárolóban, és válassza ki **PIN-kód toodashboard**.
   >
   >
3. A tárolók hello listából válassza az irányítópult hello tároló tooopen. Ha bejelöli az hello tárolóban, hello tároló irányítópult és hello **beállítások** panel megnyitása. A kép a következő hello, hello **Contoso-tároló** irányítópult ki van jelölve.

    ![Nyissa meg a tároló irányítópult és a beállítások panel](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a>Nyissa meg a tároló elem irányítópult
Hello előző eljárásban megnyitott hello tároló irányítópult. tooopen hello tároló elem irányítópult:

1. Hello tároló irányítópult hello **biztonsági mentés elemek** csempére, kattintson a **Azure virtuális gépek**.

    ![Nyissa meg a biztonsági másolati elemei csempe](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    Hello **biztonsági másolati elemei** panel felsorolja az egyes elemekhez tartozó biztonsági mentési feladat utolsó hello. Ebben a példában a rendszer egy védett virtuális gépek, demovm-markgal, amelyet ehhez a tárolóhoz.  

    ![Biztonsági másolati elemei csempe](./media/backup-azure-manage-vms/backup-items-blade.png)

   > [!TIP]
   > Könnyű elérés, a PIN-kód egy tároló elem toohello Azure irányítópult. egy tároló elem hello tároló elemek listáját, kattintson a jobb gombbal hello elemet, és válassza ki a toopin **PIN-kód toodashboard**.
   >
   >
2. A hello **biztonsági mentés elemek** panelen hello elem tooopen hello tároló elem irányítópult kattintva.

    ![Biztonsági másolati elemei csempe](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    hello tároló elem irányítópult és a **beállítások** panel megnyitása.

    ![Biztonsági másolati elemei irányítópultot, amelynek beállítások panel](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    Irányítópultról hello tároló elem például több kulcskezelési feladatok, érhető el:

   * Módosítsa a házirendek, vagy hozzon létre egy új biztonsági mentési házirend < br\>
   * megtekintheti a visszaállítási pontok, és a konzisztencia állapota < br\>
   * igény szerinti biztonsági mentést a virtuális gépek < br\>
   * Állítsa le a virtuális gépek védelme < br\>
   * a virtuális gépek a védelem folytatásához < br\>
   * a biztonsági mentési adatok (vagy a helyreállítási pont) törlése < br\>
   * [Állítsa vissza a biztonsági mentési lemezek](backup-azure-arm-restore-vms.md#restore-backed-up-disks) < br\>

Az alábbi eljárásokat hello kiindulási pontjaként hello hello tároló elem irányítópult.

## <a name="manage-backup-policies"></a>Biztonsági mentési irányelvek kezelése
1. A hello [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **összes beállítás** tooopen hello **beállítások** panelen.

    ![A biztonsági mentési házirend panel](./media/backup-azure-manage-vms/all-settings-button.png)
2. A hello **beállítások** panelen kattintson a **biztonsági mentési házirend** tooopen adott panelhez.

    Hello paneljén hello biztonsági mentési gyakoriság és megőrzési tartomány részletei láthatók.

    ![A biztonsági mentési házirend panel](./media/backup-azure-manage-vms/backup-policy-blade.png)
3. A hello **válassza ki a biztonsági mentési házirend** menüben:

   * toochange házirendek, jelöljön ki egy másik házirendet, és kattintson a **mentése**. Új házirend hello azonnal alkalmazott toohello tárolóban. < br\>
   * toocreate egy házirendet, válassza ki **hozzon létre új**.

     ![Virtuális gép biztonsági mentése](./media/backup-azure-manage-vms/backup-policy-create-new.png)

     A biztonsági mentési házirend létrehozásával kapcsolatos utasításokért lásd: [biztonsági mentési házirend meghatározása](backup-azure-manage-vms.md#defining-a-backup-policy).

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

> [!NOTE]
> Miközben a biztonsági mentési házirendek kezelése, győződjön meg arról, hogy toofollow hello [ajánlott eljárások](backup-azure-vms-introduction.md#best-practices) optimális teljesítményének eléréséhez biztonsági mentés
>
>

## <a name="on-demand-backup-of-a-virtual-machine"></a>Igény szerinti biztonsági mentést a virtuális gépek
Igény szerinti biztonsági mentését egy virtuális gép eltarthat, ha konfigurálva van a védelem. Ha hello kezdeti biztonsági mentés folyamatban, igény szerinti biztonsági mentést a Recovery Services-tároló hello hello virtuális gép teljes másolatot készít. Hello kezdeti biztonsági mentésének végrehajtását, ha egy igény szerinti biztonsági mentést csak küldése módosítások hello korábbi pillanatképből, toohello Recovery Services-tároló. Ez azt jelenti, hogy azt követő biztonsági mentéseket a rendszer mindig növekményes.

> [!NOTE]
> egy igény szerinti biztonsági mentés megőrzési időtartama hello hello napi biztonsági mentési pontok hello házirendben megadott hello megőrzési idő értékét. Ha nincs napi biztonsági mentési pont van kijelölve, hello heti biztonsági mentési pont szerepel.
>
>

tootrigger igény szerinti biztonsági mentést a virtuális gépek:

* A hello [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **biztonsági mentés most**.

    ![Biztonsági mentés most gombra.](./media/backup-azure-manage-vms/backup-now-button.png)

    hello portal gondoskodik arról, hogy szeretné-e toostart egy igény szerinti biztonsági mentési feladat. Kattintson a **Igen** toostart hello biztonsági mentési feladat.

    ![Biztonsági mentés most gombra.](./media/backup-azure-manage-vms/backup-now-check.png)

    hello biztonsági mentési feladat létrehoz egy helyreállítási pontot. hello helyreállítási pont megőrzési időtartamának hello van hello ugyanaz, mint a hello virtuális géphez társított hello házirendben megadott megőrzési ideig. tootrack hello folyamatban hello feladat hello tároló irányítópulton kattintson hello **biztonsági mentési feladatok** csempére.  

## <a name="stop-protecting-virtual-machines"></a>Virtuális gépek védelmének megszüntetése
Ha úgy dönt, hogy a virtuális gépek védelmének toostop, a program kéri, ha azt szeretné, tooretain hello helyreállítási pontokat. Két módon toostop védelmet nyújtó virtuális gépek:

* Állítsa le az összes jövőbeli biztonsági mentési feladat, és törölje az összes helyreállítási pont, vagy
* Állítsa le az összes jövőbeli biztonsági mentési feladat, de hagyjon hello helyreállítási pontok <br/>

Nincs társított helyreállítási pontok hello hagyja a tárolási költség. Azonban hello hello Kilépés a helyreállítási pontok előnye, később visszaállíthatja hello virtuális gép igény. Hello költség információt, hogy hello a helyreállítási pontok, lásd: hello [díjszabása](https://azure.microsoft.com/pricing/details/backup/). Ha úgy dönt, toodelete összes helyreállítási pont, hello virtuális gép nem állítható vissza.

a virtuális gép toostop védelmét:

1. A hello [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **Stop biztonsági mentés**.

    ![Állítsa le a biztonsági mentési gomb](./media/backup-azure-manage-vms/stop-backup-button.png)

    hello állítsa le a biztonsági mentés panel nyílik meg.

    ![Állítsa le a biztonsági mentési panel](./media/backup-azure-manage-vms/stop-backup-blade.png)
2. A hello **biztonsági mentés leállítása** panelen válassza ki, hogy tooretain vagy delete hello biztonsági mentési adatokat. hello információ írja be a választott ismerteti.

    ![Állítsa le a védelmet](./media/backup-azure-manage-vms/retain-or-delete-option.png)
3. Ha úgy dönt, hogy tooretain hello biztonsági mentési adatokat, akkor hagyja ki toostep 4. Ha úgy dönt, hogy a biztonsági mentési adatok toodelete, győződjön meg arról, hogy szeretné, hogy toostop hello biztonsági mentési feladatot, és hello helyreállítási pontok – hello típusnév hello elem törlése.

    ![Állítsa le a ellenőrzése](./media/backup-azure-manage-vms/item-verification-box.png)

    Ha nem biztos a hello elem neve, mutasson hello felkiáltójel tooview hello nevét. Hello elem hello nevét is a **állítsa le a biztonsági mentés** hello panel hello tetején.
4. Megadhat egy **OK** vagy **Megjegyzés**.
5. Kattintson a toostop hello biztonsági mentési feladat az aktuális elem hello ![leállítás biztonsági mentés gombra.](./media/backup-azure-manage-vms/stop-backup-button-blue.png)

    Értesítési üzenet értesíti Önt arról a hello biztonsági mentési feladatot leállították.

    ![Állítsa le a védelmet megerősítése](./media/backup-azure-manage-vms/stop-message.png)

## <a name="resume-protection-of-a-virtual-machine"></a>A virtuális gépek a védelem folytatásához
Ha hello **biztonsági mentési adatok megőrzése** lehetőség választása amikor hello virtuális gép védelmét le lett állítva, akkor célszerű lehetséges tooresume védelem. Ha hello **biztonságimásolat-adatok törlése** lehetőséget választotta, akkor nem lehet folytatni a hello virtuális gép védelmét.

hello virtuális gép védelmét tooresume

1. A hello [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **biztonsági mentés folytatása**.

    ![A védelem folytatásához](./media/backup-azure-manage-vms/resume-backup-button.png)

    hello biztonsági szabályzat panel nyílik meg.

   > [!NOTE]
   > Hello virtuális gép ismételt védelmével, amikor dönthet úgy, mint hello házirend, amellyel a virtuális gép védett eredetileg másik szabályt.
   >
   >
2. Hello kövesse [biztonsági mentési házirendek kezelése](backup-azure-manage-vms.md#manage-backup-policies) tooassign hello házirend hello virtuális géphez.

    Ha biztonsági mentési házirend hello alkalmazott toohello virtuális gépet, a következő üzenet hello láthatja.

    ![Sikeresen védett virtuális gép](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Biztonságimásolat-adatok törlése
Hello hello során a virtuális géphez tartozó biztonsági mentési adatok törlése **Stop biztonsági mentés** feladat, vagy bármikor hello után a biztonsági mentési feladat befejeződött. Akkor lehet hasznos toowait nap vagy hét hello helyreállítási pontjainak törlése előtt. Ellentétben a helyreállítási pontok visszaállítása a biztonsági mentési adatok törlésekor, adott helyreállítási pontok toodelete nem választható. Ha úgy dönt, toodelete a biztonsági mentési adatokat, hello elemhez tartozó összes helyreállítási pont törlése.

hello következő eljárás azt feltételezi, hogy hello biztonságimásolat-készítő feladat hello virtuális gép leállítása vagy letiltása. Hello biztonságimásolat-készítő feladat le van tiltva, miután hello **biztonsági mentés folytatása** és **Delete biztonsági mentés** az hello tároló elem irányítópult érhetők el.

![Folytatás és a Törlés gombbal](./media/backup-azure-manage-vms/resume-delete-buttons.png)

biztonsági mentési adatok toodelete hello egy virtuális gépen *biztonsági mentése le van tiltva*:

1. A hello [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **Delete biztonsági mentés**.

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    Hello **biztonságimásolat-adatok törlése** panel nyílik meg.

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/delete-backup-blade.png)
2. Hello típusnév hello elem tooconfirm kívánt toodelete hello helyreállítási pontokat.

    ![Állítsa le a ellenőrzése](./media/backup-azure-manage-vms/item-verification-box.png)

    Ha nem biztos a hello elem neve, mutasson hello felkiáltójel tooview hello nevét. Hello elem hello nevét is a **biztonságimásolat-adatok törlése** hello panel hello tetején.
3. Megadhat egy **OK** vagy **Megjegyzés**.
4. Kattintson a toodelete hello biztonsági mentési adatok az aktuális elem hello ![leállítás biztonsági mentés gombra.](./media/backup-azure-manage-vms/delete-button.png)

    Értesítési üzenet értesíti Önt arról a hello biztonsági mentési adatok törölve lett.

## <a name="next-steps"></a>Következő lépések
Hozza létre újra a virtuális gép helyreállítási pontból információkért tekintse meg [visszaállítása az Azure virtuális gépek](backup-azure-restore-vms.md). Ha a virtuális gépek védelméről tájékoztatásra van szüksége, tekintse meg [először: készítsen biztonsági mentést a Recovery Services-tároló virtuális gépek tooa](backup-azure-vms-first-look-arm.md). Események figyelésével kapcsolatos további információkért lásd: [figyelése az Azure virtuális gépek biztonsági mentéseinek riasztások](backup-azure-monitor-vms.md).
