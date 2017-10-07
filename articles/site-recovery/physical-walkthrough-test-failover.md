---
title: "a fizikai kiszolgáló replikációs tooAzure az Azure Site Recovery feladatátvételi teszt aaaRun |} Microsoft Docs"
description: "Összefoglalja a [fizikai kiszolgálók replikálása tooAzure hello Azure Site Recovery szolgáltatással a feladatátvételi teszt futtatásához szükséges hello lépést."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a640e139-3a09-4ad8-8283-8fa92544f4c6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: f8ed5ce585c5574be3018ce15339c4fd3b527eaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-11-run-a-test-failover-of-physical-servers-tooazure"></a>11. lépés: A fizikai kiszolgálók tooAzure feladatátvételi teszt futtatása

Ez a cikk ismerteti, hogyan toorun a feladatátvételi tesztet a helyszíni fizikai kiszolgálók tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Előkészületek

Javasoljuk, hogy ellenőrizze hello kiszolgáló tulajdonságait, és hajtsa végre a módosításokat feladatátvételi teszt futtatása előtt kell. van-e hozzáférési hello Virtuálisgép-tulajdonságokat **replikált elemek**. Hello **Essentials** panel virtuálisgép-beállításainak és állapotával kapcsolatos adatokat jeleníti meg.

## <a name="managed-disk-considerations"></a>Felügyelt lemezzel kapcsolatos megfontolások

[Által kezelt lemezeken](../virtual-machines/windows/managed-disks-overview.md) egyszerűsítheti az Azure virtuális gépeken, a Lemezkezelés eszköz hello virtuális gépek lemezei társított hello storage-fiókok kezelése. 

- Egy kiszolgáló védelmének engedélyezésekor a virtuális gép adatait replikálja tooa tárfiók. Felügyelt lemezek jönnek létre, és toohello VM csatolt, csak akkor, ha feladatátvétel történik.
- Felügyelt lemezek csak Azure virtuális gépek telepíthető hello Resource Manager-modell használatával hozható létre.  
- Ez a beállítás engedélyezése esetén csak a rendelkezésre állási beállítja tartalmazó erőforráscsoportokat **az kezelt lemezek** engedélyezve választható ki. Felügyelt lemezzel rendelkező virtuális gépek rendelkezésre állási készletek kell **az kezelt lemezek** túl beállítása**Igen**. Ha hello beállítás nincs engedélyezve a virtuális gépeket, majd csak lévő rendelkezésre állási csoportokra erőforráscsoportok nélkül engedélyezve kezelt lemezek választható ki.
- [További](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set) felügyelt lemezek és a rendelkezésre állási állítja be.
- A Storage szolgáltatás titkosítási Titkosított hello replikáláshoz használni kívánt tárfiókot, kezelt lemezek nem hozható létre, a feladatátvétel során. Ebben az esetben vagy nem felügyelt lemezek használatának engedélyezése vagy tiltsa le a védelmet a virtuális gép hello és engedélyezéshez tegye toouse egy tárfiókot, amelyekkel engedélyezhető a titkosítás nem rendelkezik. [További információk](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="network-considerations"></a>Hálózati kapcsolatos szempontok

Egy Azure virtuális gép a feladatátvételt követően létrehozott beállíthat hello cél IP-címet.

- Ha nem ad meg egy címet, a gép a feladatátvételt hello DHCP fogja használni.
- Ha egy címet, amely nem használható feladatátadásra, feladatátvétel nem fog működni.
- cél IP-cím a feladatátvételi teszthez, használható, ha hello címkészletében nem áll rendelkezésre a feladatátvételi teszt hálózatában hello hello.
- hálózati adapterek száma hello hello cél virtuális gép számára megadott hello mérete határozza meg:

     - Ha hálózati adapterek hello forrásgépen hello száma hello ugyanaz, mint a, vagy kevesebb, mint hello hello célként megadott gép méretéhez engedélyezett adapterek számával, akkor hello cél lesz hello ugyanannyi adapter hello forrásaként.
     - Ha hello hello forrás virtuális gép adaptereinek száma meghaladja hello engedélyezett hello célméretet, akkor hello cél maximális használja a rendszer.
     - Például ha a forrásgépen két hálózati adaptert, és hello célgép mérete négy támogatja, akkor hello célgépen két adapter fog működni. Ha hello forrásgépen két adapter, azonban a hello támogatott célméretet csak egy majd hello célgépen csak egy adapter fog működni.     
   - Ha a virtuális gépnek több hálózati adapter hello lesz az összes csatlakoznak toohello ugyanazon a hálózaton.
   - Ha hello a virtuális gépnek a több hálózati adapter, majd hello először egy hello listán látható lesz hello *alapértelmezett* hello Azure virtuális gép hálózati adapteréhez.
 - [További](vmware-walkthrough-network.md) kapcsolatos IP-címzést.



## <a name="view-and-modify-vm-settings"></a>Virtuális gép beállítások megtekintése és módosítása

Azt javasoljuk, hogy ellenőrizze hello tulajdonságok hello forráskiszolgáló, a feladatátvétel futtatása előtt.

1. A **védett elemek**, kattintson a **replikált elemek**, és kattintson a hello gépre.
2. A hello **replikált elemek** ablaktáblában láthatja a gép adatait, állapotát és hello legújabb elérhető helyreállítási pontok összegzését. Kattintson a **tulajdonságok** tooview további részletek.
3. A **számítás és hálózat**, akkor is:
    - Hello Azure virtuális gép nevének módosításához. hello nevét meg kell felelnie [Azure-követelményeknek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
    - Adja meg a feladatátvételt követően [erőforráscsoport].
    - Adja meg a célméretet hello Azure virtuális Gépen
    - Válasszon egy [rendelkezésre állási csoport](../virtual-machines/windows/tutorial-availability-sets.md).
    - Adja meg, hogy toouse [által kezelt lemezeken](#managed-disk-considerations). Válassza ki **Igen**, ha azt szeretné, hogy tooattach felügyelt lemezek tooyour gép áttelepítési tooAzure.
    - Megtekintése vagy módosítása a hálózati beállítások, például hello hálózatot/alhálózatot mely hello az Azure virtuális gép található a feladatátvételt követően és hello IP-címet, amely tooit hozzá lesz rendelve.
4. A **lemezek**, hello operációs rendszerre vonatkozó adatokat megtekintheti, és az adatlemezek hello virtuális gép.

## <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása

Miután minden állított be, futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik.

- Ha azt szeretné, hogy a virtuális gépek tooconnect tooAzure, feladatátvételt követően RDP segítségével [tooconnect előkészítése](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - az Active Directory és a DNS-toocopy van szüksége a tesztkörnyezetben toofully tesztje. [További információk](site-recovery-active-directory.md#test-failover-considerations).
 - Teljes információ a feladatátvételi teszt [Ez a cikk](site-recovery-test-failover-to-azure.md) cikk.
- Gyorsan áttekintheti videó elindítása előtt:

     
     >[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]

Most a feladatátvétel futtatása:

1. toofail keresztül egyetlen számítógépen, a **beállítások** > **replikált elemek**, kattintson a hello gépre > **+ feladatátvételi teszt** ikonra.

    ![A feladatátvételi teszt](./media/physical-walkthrough-test-failover/test-failover.png)

2. a helyreállítás alatt toofail tervez, a **beállítások** > **helyreállítási tervek**, kattintson a jobb gombbal hello terv > **feladatátvételi teszt**. a helyreállítási terv toocreate [kövesse ezeket az utasításokat](site-recovery-create-recovery-plans.md).  

3. A **feladatátvételi teszt**, jelölje be hello Azure hálózati toowhich Azure virtuális gépek csatlakoznak feladatátvételt követően.

4. Kattintson a **OK** toobegin hello feladatátvételi. Előrehaladásának parancsával hello gép tooopen tulajdonságát, vagy a hello **feladatátvételi teszt** feladatot a tároló neve > **beállítások** > **feladatok**  >  **Site Recovery-feladatok**.

5. Hello feladatátvétel befejezése után meg kell tudni toosee hello replika Azure virtuális gép hello Azure-portálon jelenik meg > **virtuális gépek**. Győződjön meg arról, hogy a virtuális gép hello hello megfelelő méretét, csatlakoztatva van a megfelelő hálózati toohello, és fut-e.

6. Ha elvégezte a feladatátvételt követően, meg kell tudni tooconnect toohello Azure virtuális Gépen.

### <a name="delete-test-failover-vms"></a>A feladatátvételi teszt virtuális gépek törlése

1. Befejezése után kattintson a **karbantartása a feladatátvételi teszt** hello helyreállítási terv vagy a gépen.
2. A **megjegyzések**, és a feladatátvételi teszt hello kapcsolatos megfigyelések feljegyzéséhez mentéséhez.
3. hello törlési művelet törli a teszt feladatátvétele során létrehozott Azure virtuális gépeken.

## <a name="summary"></a>Összefoglalás

Ha hello a feladatátvételi teszt sikeresen befejeződött, a fizikai kiszolgálókat replikál, és ellenőrizte, hogy azok a feladatátvétel tooAzure. Most a feladatátvételeket is futtathatja a szervezeti követelményeknek megfelelően. 

Ne feledje, hogy nem jelenleg utasíthat el újból az Azure tooa fizikai kiszolgálón. VMware virtuális gép tooa biztonsági toofail rendelkezik. Ez azt jelenti, hogy egy helyszíni VMware-infrastruktúra rendelés toofail vissza van szüksége. [További](site-recovery-failback-azure-to-vmware.md) kapcsolatos hátsó Azure virtuális gépek tooVMware sikertelenek lesznek.


## <a name="next-steps"></a>Következő lépések

- [Feladatátadását](site-recovery-failover.md) igény szerint.
