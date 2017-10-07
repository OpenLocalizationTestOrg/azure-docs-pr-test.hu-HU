---
title: "VMware virtuális gépek replikálása az Azure Site Recovery tooAzure aaaEnable replikálása |} Microsoft Docs"
description: "VMware virtuális gépek hello Azure Site Recovery szolgáltatással tooenable replikációs tooAzure szüksége hello lépéseket foglalja"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 519c5559-7032-4954-b8b5-f24f5242a954
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 490782bbbfa3dd92c626d3985c75d771df53d566
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-11-enable-replication-for-vmware-virtual-machines-tooazure"></a>11. lépés: A VMware virtuális gépek tooAzure replikáció engedélyezése


Ez a cikk ismerteti, hogyan tooenable replikálása a helyszíni VMware virtuális gépek tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Előkészületek

- VMware virtuális gépek rendelkeznie kell hello [telepíteni a mobilitási szolgáltatás összetevőt](vmware-walkthrough-install-mobility.md). – Ha egy virtuális gép kész az ügyfélleküldéses telepítést, a hello folyamatkiszolgáló automatikusan telepíti az hello mobilitási szolgáltatás, amikor a replikáció engedélyezése.
- Az Azure felhasználói fiókot kell adott [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) egy virtuális gép tooAzure tooenable replikálása
- Hozzáadásakor, vagy módosítsa a virtuális gépek, is igénybe vehet fel too15 perc vagy már módosítások tootake hatás és a számukra tooappear hello portálon.
- A legutóbb felfedezett hello idő ellenőrizze, hogy a virtuális gépek **konfigurációs kiszolgálók** > **utolsó lépjen kapcsolatba a**.
- virtuális gépek tooadd hello ütemezett felderítési, kiemelési hello konfigurációs kiszolgáló várakozás nélkül (ne kattintson azt), és kattintson a **frissítése**.



## <a name="exclude-disks-from-replication"></a>Lemezek kizárása a replikációból

Alapértelmezés szerint az összes lemez gépen replikálódnak. Lemezek lehet kizárni a replikálásból. Például előfordulhat, hogy nem kívánja tooreplicate lemezek, amelyek ideiglenes vagy frissítette minden alkalommal, amikor olyan gép, adatok, vagy alkalmazás újraindul, (például pagefile.sys vagy SQL Server tempdb). [További információ](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a>Virtuális gépek replikálása

Mielőtt nekikezdene, tekintse meg a gyors áttekintő videó

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. Kattintson **2. lépés: Az alkalmazás replikálása** > **Forrás** elemre.
2. A **forrás**, jelölje be hello konfigurációs kiszolgáló.
3. A **számítógép típusú**, jelölje be **virtuális gépek**.
4. A **vCenter vagy vSphere Hipervizorra**, hello vCenter-kiszolgálót, amely hello vSphere-gazdagép felügyeli, vagy válasszon hello állomás.
5. Hello folyamatkiszolgáló kijelölése. Ha még nem hozott létre minden olyan további folyamat kiszolgálóhoz ez lesz hello konfigurációs kiszolgáló. Ezután kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/vmware-walkthrough-enable-replication/enable-replication2.png)

6. A **cél**válassza ki a hello előfizetés, hello virtuális gépek a feladatátvételt toocreate hello használni kívánt erőforráscsoportot. Válassza ki a beállítani kívánt toouse (klasszikus vagy erőforrás-kezelés), Azure virtuális gépek a feladatátvételt hello hello telepítési modell.


7. Válassza ki a kívánt toouse adatok replikálásához hello Azure storage-fiók. Ha már beállította fiók toouse nem szeretné, létrehozhat egy újat.

8. SELECT hello Azure hálózati és alhálózati toowhich Azure virtuális gépeken fog csatlakozni, ha a feladatátvételt követően jönnek létre. Válassza ki **beállítás most a kijelölt gépekhez**, tooapply hello beállítás tooall számítógépek választja a védelem. Válassza ki **konfigurálását később** tooselect hello gépenként Azure-hálózatot. Ha egy meglévő hálózati toouse nem szeretné, akkor hozzon létre egyet.

    ![A replikáció engedélyezése](./media/vmware-walkthrough-enable-replication/enable-rep3.png)
9. A **virtuális gépek** > **válassza ki a virtuális gépek**kattintson, és válassza az egyes gépek tooreplicate szeretné. Csak olyan gépeket választhat, amelyeken használható a replikáció funkció. Végül kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/vmware-walkthrough-enable-replication/enable-replication5.png)
10. A **tulajdonságok** > **tulajdonságainak konfigurálása**, jelölje be hello folyamat server tooautomatically által használt fiók hello hello mobilitási szolgáltatás telepítése hello gépen.
11. Alapértelmezés szerint a rendszer replikálja az összes lemez. Kattintson a **összes lemez** , és törölje a nem kívánt tooreplicate lemezzel. Ezután kattintson az **OK** gombra. Virtuális gép további tulajdonságokat később állíthatja be.

    ![A replikáció engedélyezése](./media/vmware-walkthrough-enable-replication/enable-replication6.png)
11. A **replikációs beállítások** > **replikáció beállításainak konfigurálása**, győződjön meg arról, hogy helyes-e replikációs házirend be van jelölve hello. Ha módosít egy házirendet, a változások lesznek alkalmazott tooreplicating gép és toonew gépek.
12. Engedélyezése **virtuális Gépre kiterjedő konzisztencia** Ha toogather gépeihez egy replikációs csoporthoz, és adjon meg egy nevet hello csoportnak. Ezután kattintson az **OK** gombra. Vegye figyelembe:

    * A replikációs csoportok gépek replikálása együtt, és megosztott összeomlás-konzisztens és alkalmazáskonzisztens helyreállítási pontokat, amikor azok a feladatátvételt.
    * Azt javasoljuk, hogy gyűjtse össze a virtuális gépek és fizikai kiszolgálók együtt, hogy látni lehessen a munkaterhelések. Több virtuális Gépre kiterjedő konzisztencia engedélyezése befolyásolhatja a teljesítményt a munkaterhelés, és csak akkor használja, ha a gépek is működnek hello azonos munkaterhelés, és konzisztenciára van szükség.

    ![A replikáció engedélyezése](./media/vmware-walkthrough-enable-replication/enable-replication7.png)
13. Kattintson a **engedélyezze a replikálást**. Hello állapotának nyomon követheti **Védelemengedélyezési** feladat **beállítások** > **feladatok** > **Site Recovery-feladatok**. Hello után **Védelemvéglegesítési** feladat futtatása hello gép készen áll a feladatátvételre.

## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[12. lépés: feladatátvételi teszt futtatása](vmware-walkthrough-test-failover.md)
