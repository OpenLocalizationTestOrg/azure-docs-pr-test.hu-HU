---
title: "a Hyper-V replikáció (a System Center VMM nélkül) tooAzure feladatátvételi teszt aaaRun |} Microsoft Docs"
description: "A feladatátvételi teszt futtatása Hyper-V virtuális gépek replikálása hello Azure Site Recovery szolgáltatással tooAzure szüksége hello lépéseket foglalja össze."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c9a4c3ca-84a0-4668-b700-9b0046202299
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: dbcca080a8fa139e2ea0d132323101dd0f7cd265
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-11-run-a-test-failover-for-hyper-v-replication-tooazure"></a>11. lépés: Hyper-V replikáció tooAzure a feladatátvételi teszt futtatása

Ez a cikk ismerteti, hogyan toorun a feladatátvételi teszt helyszíni Hyper-V virtuális gépek (System Center VMM által nem felügyelt) tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Előkészületek

Javasoljuk, hogy ellenőrizze hello virtuális gép tulajdonságait, és hajtsa végre a módosításokat feladatátvételi teszt futtatása előtt kell. van-e hozzáférési hello Virtuálisgép-tulajdonságokat **replikált elemek**. Hello **Essentials** panel gépek beállítások és állapotával kapcsolatos adatokat jeleníti meg.

## <a name="managed-disk-considerations"></a>Felügyelt lemezzel kapcsolatos megfontolások

[Által kezelt lemezeken](../virtual-machines/windows/managed-disks-overview.md) egyszerűsítheti az Azure virtuális gépeken, a Lemezkezelés eszköz hello virtuális gépek lemezei társított hello storage-fiókok kezelése. 

- Felügyelt lemezek jönnek létre, és toohello VM csatolt, csak akkor, ha egy feladatátvevő tooAzure következik be. Ha engedélyezi a védelmet, a helyszíni virtuális gépek adatait replikálja toostorage fiókok.
- Felügyelt lemezek csak hello Resource manager üzembe helyezési modellben rel központilag telepített virtuális gépek hozható létre.
- Feladat-visszavétel Azure tooan a helyszíni Hyper-V környezet jelenleg nem támogatott a felügyelt lemezek gépek. Csak akkor kell beállítania **az kezelt lemezek** túl**Igen** Ha egy áttelepítési csak (feladatátvételi tooAzure nélkül feladat-visszavétel) végez
- Ez a beállítás engedélyezése esetén csak a rendelkezésre állási beállítja tartalmazó erőforráscsoportokat **az kezelt lemezek** engedélyezve választható ki. Felügyelt lemezzel rendelkező virtuális gépek rendelkezésre állási készletek kell **az kezelt lemezek** túl beállítása**Igen**. Ha hello beállítás nincs engedélyezve a virtuális gépeket, majd csak lévő rendelkezésre állási csoportokra erőforráscsoportok nélkül engedélyezve kezelt lemezek választható ki. [További információk](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).
- - A Storage szolgáltatás titkosítási Titkosított hello replikáláshoz használni kívánt tárfiókot, kezelt lemezek nem hozható létre, a feladatátvétel során. Ebben az esetben vagy nem felügyelt lemezek használatának engedélyezése vagy tiltsa le a védelmet a virtuális gép hello és engedélyezéshez tegye toouse egy tárfiókot, amelyekkel engedélyezhető a titkosítás nem rendelkezik. [További információk](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).

 
## <a name="network-considerations"></a>Hálózati kapcsolatos szempontok
    
- Hello cél IP cím toobe használt hello Azure virtuális gép a feladatátvételt követően is beállíthatja. Ha nem ad meg egy címet, a gép a feladatátvételt hello DHCP fogja használni. Ha egy címet, amely nem használható feladatátadásra, hello feladatátvétel sikertelen lesz. cél IP-cím a feladatátvételi teszthez használható, ha hello címkészletében nem áll rendelkezésre a feladatátvételi teszt hálózatában hello hello.
- hálózati adapterek száma hello hello mérete határozza meg hello cél virtuális gépre, az alábbiak szerint határozza meg:
    - Ha hálózati adapterek hello forrásgépen hello száma kisebb vagy egyenlő, mint hello cél gép méretéhez engedélyezett adapterek toohello számát, majd hello cél lesz hello ugyanannyi adapter hello forrásaként.
    - Ha hello hello forrás virtuális gép adaptereinek száma meghaladja hello engedélyezett hello célméretet majd hello cél maximális fogja használni.
    - Ha például a forrásgépen két hálózati adapterrel és hello célgép mérete négy támogatja, hello célgépen két adapter fog működni. Ha hello forrásgépen két adapter, azonban a hello támogatott célméretet csak egy majd hello célgépen csak egy adapter fog működni.     
- Ha hello virtuális gép több hálózati adapter lesz összes csatlakozáshoz toohello ugyanazon a hálózaton.
- Ha hello a virtuális gépnek a több hálózati adapter, majd hello először egy hello listán látható lesz hello *alapértelmezett* hello Azure virtuális gép hálózati adapteréhez.


## <a name="view-and-manage-vm-settings"></a>Megtekintheti és kezelheti a virtuális gép beállításait

Azt javasoljuk, hogy ellenőrizze hello forrásgép hello tulajdonságai, a feladatátvétel futtatása előtt.

1. A **védett elemek**, kattintson a **replikált elemek**, és kattintson a virtuális gép hello.

    ![A replikáció engedélyezése](./media/hyper-v-site-walkthrough-test-failover/test-failover1.png)
2. A hello **replikált elemek** panelen megtekintheti a virtuális gép állapotát, és hello legújabb elérhető helyreállítási pontok összegzését. Kattintson a **tulajdonságok** tooview további részletek.

    ![A replikáció engedélyezése](./media/hyper-v-site-walkthrough-test-failover/test-failover2.png)
3. A **számítás és hálózat**, akkor is:
    - Hello Azure virtuális gép nevének módosításához. hello nevét meg kell felelnie [Azure-követelményeknek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
    - Adja meg a feladatátvételt követően [erőforráscsoport].
    - Adja meg a célméretet hello Azure virtuális Gépen
    - Válasszon egy [rendelkezésre állási csoport](../virtual-machines/windows/tutorial-availability-sets.md).
    - Adja meg, hogy toouse [által kezelt lemezeken](#managed-disk-considerations). Válassza ki **Igen**, ha azt szeretné, hogy tooattach felügyelt lemezek tooyour gép áttelepítési tooAzure.
    - Megtekintése vagy módosítása a hálózati beállítások, például hello hálózatot/alhálózatot mely hello az Azure virtuális gép található a feladatátvételt követően és hello IP-címet, amely tooit hozzá lesz rendelve.

    ![A replikáció engedélyezése](./media/hyper-v-site-walkthrough-test-failover/test-failover4.png)
4. A **lemezek**, hello operációs rendszerre vonatkozó adatokat megtekintheti, és az adatlemezek hello virtuális gép.


## <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása

Futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik.

- Ha azt szeretné, hogy a virtuális gépek tooconnect tooAzure, feladatátvételt követően RDP segítségével [tooconnect előkészítése](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - az Active Directory és a DNS-toocopy van szüksége a tesztkörnyezetben toofully tesztje. [További információk](site-recovery-active-directory.md#test-failover-considerations).
 - Teljes információ a feladatátvételi teszt [Ez a cikk](site-recovery-test-failover-to-azure.md) cikk.
 
 Most feladatátvételt:

1. toofail keresztül egyetlen számítógépen, a **replikált elemek**, kattintson a virtuális gép hello > **+ feladatátvételi teszt** ikonra.
2. a helyreállítás alatt toofail tervez, a **helyreállítási tervek**, kattintson a jobb gombbal hello terv > **feladatátvételi teszt**. a helyreállítási terv toocreate [kövesse ezeket az utasításokat](site-recovery-create-recovery-plans.md).
3. A **feladatátvételi teszt**, jelölje be hello Azure hálózati toowhich Azure virtuális gépek csatlakoznak feladatátvételt követően.
4. Kattintson a **OK** toobegin hello feladatátvételi. Előrehaladásának parancsával hello VM tooopen tulajdonságát, vagy a hello **feladatátvételi teszt** feladatot a tároló neve > **feladatok** > **Site Recovery-feladatok**.
5. Hello feladatátvétel befejezése után meg kell jelennie toosee hello replika Azure machine hello Azure-portálon jelenik meg > **virtuális gépek**. Győződjön meg arról, hogy a virtuális gép hello hello megfelelő méretét, csatlakoztatva van a megfelelő hálózati toohello, és fut-e.
6. Ha elvégezte a feladatátvételt követően, meg kell tudni tooconnect toohello Azure virtuális Gépen.
7. Ha befejezte, kattintson a **karbantartása a feladatátvételi teszt** a hello helyreállítási terv. A **megjegyzések** és hello feladatátvételi teszthez kapcsolatos megfigyelések feljegyzéséhez mentéséhez. Ezzel a lépéssel törli a teszt feladatátvétele során létrehozott hello virtuális gépek.



## <a name="next-steps"></a>Következő lépések

- [További](site-recovery-failover.md) kapcsolatos különféle a feladatátvételeket, és hogyan toorun őket.
- [További információ a feladat-visszavétel](site-recovery-failback-from-azure-to-hyper-v.md), toofail vissza és replikálása Azure virtuális gépek biztonsági toohello elsődleges helyszíni Hyper-V helyet.

