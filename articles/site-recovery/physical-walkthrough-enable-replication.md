---
title: "a fizikai kiszolgálók replikálása az Azure Site Recovery tooAzure aaaEnable replikációs |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket kell tooenable replikációs tooAzure fizikai kiszolgálók hello Azure Site Recovery szolgáltatással"
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
ms.openlocfilehash: dde4b1463023d2ccefa498f72bb51e57d60ac0d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-enable-replication-for-physical-servers-tooazure"></a>10. lépés: A fizikai kiszolgálók tooAzure replikáció engedélyezése


Ez a cikk ismerteti, hogyan tooenable replikálása a helyszíni windowsos/Linuxos fizikai kiszolgálók tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Előkészületek

- Kiszolgálóknak rendelkezniük kell hello [telepíteni a mobilitási szolgáltatás összetevőt](physical-walkthrough-install-mobility.md).
- A virtuális gépek kész az ügyfélleküldéses telepítést, ha a hello folyamatkiszolgáló automatikusan telepíti az hello mobilitási szolgáltatás, amikor a replikáció engedélyezése.
- Ha engedélyezi a gép a replikáció, a Azure felhasználói fiókot kell adott [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooensure képes toouse az Azure storage, és hozzon létre az Azure virtuális gépeken.
- Amikor fel vagy módosít a kiszolgálók IP-címeit, is igénybe vehet fel too15 perc vagy már módosítások tootake hatása, valamint azokat a tooappear hello portálon.


## <a name="exclude-disks-from-replication"></a>Lemezek kizárása a replikációból

Alapértelmezés szerint az összes lemez gépen replikálódnak. Lemezek lehet kizárni a replikálásból. Például előfordulhat, hogy nem kívánja tooreplicate lemezek, amelyek ideiglenes vagy frissítette minden alkalommal, amikor olyan gép, adatok, vagy alkalmazás újraindul, (például pagefile.sys vagy SQL Server tempdb). [További információ](site-recovery-exclude-disk.md)

## <a name="replicate-servers"></a>Kiszolgálók replikálása

1. Kattintson **2. lépés: Az alkalmazás replikálása** > **Forrás** elemre.
2. A **forrás**, jelölje be hello konfigurációs kiszolgáló.
3. A **számítógép típusú**, jelölje be **fizikai gépek**.
4. Hello folyamatkiszolgáló kijelölése. Ha még nem hozott létre minden olyan további folyamat kiszolgálóhoz ez lesz hello konfigurációs kiszolgáló. Ezután kattintson az **OK** gombra.
5. A **cél**válassza ki a hello előfizetés, hello virtuális gépek a feladatátvételt toocreate hello használni kívánt erőforráscsoportot. Válassza ki a beállítani kívánt toouse (klasszikus vagy erőforrás-kezelés), Azure virtuális gépek a feladatátvételt hello hello telepítési modell.
6. Válassza ki a kívánt toouse adatok replikálásához hello Azure storage-fiók. Ha már beállította fiók toouse nem szeretné, létrehozhat egy újat.
7. Jelölje be hello **Azure hálózati** és **alhálózati** toowhich Azure virtuális gépek a feladatátvételt követően csatlakozhatnak. Válassza ki **beállítás most a kijelölt gépekhez** tooapply hello beállítás tooall számítógépek választja a védelem. Válassza ki **konfigurálását később** tooselect hello gépenként Azure-hálózatot. Ha egy meglévő hálózati toouse nem szeretné, akkor hozzon létre egyet.

    ![A replikáció engedélyezése](./media/physical-walkthrough-enable-replication/targetsettings.png)

8. A **fizikai gépek**, kattintson a **+ a fizikai gép** , és írja be a hello **neve** és **IP-cím**. Válassza ki az operációs rendszer hello hello gép tooreplicate szeretné. Amíg a számítógépek felderítése és hello listán látható néhány percet vesz igénybe.
9. A **tulajdonságok** > **tulajdonságainak konfigurálása**, jelölje be hello folyamat server tooautomatically által használt fiók hello hello mobilitási szolgáltatás telepítése hello gépen.
10. Alapértelmezés szerint a rendszer replikálja az összes lemez. Kattintson a **összes lemez** , és törölje a nem kívánt tooreplicate lemezzel. Ezután kattintson az **OK** gombra. Virtuális gép további tulajdonságokat később állíthatja be.

    ![A replikáció engedélyezése](./media/physical-walkthrough-enable-replication/enable-replication6.png)
11. A **replikációs beállítások** > **replikáció beállításainak konfigurálása**, győződjön meg arról, hogy helyes-e replikációs házirend be van jelölve hello. Ha módosít egy házirendet, a változások lesznek alkalmazott tooreplicating gép és toonew gépek.
12. Engedélyezése **virtuális Gépre kiterjedő konzisztencia** Ha toogather gépeihez egy replikációs csoporthoz, és adjon meg egy nevet hello csoportnak. Ezután kattintson az **OK** gombra. Vegye figyelembe:

    * A replikációs csoportok gépek replikálása együtt, és megosztott összeomlás-konzisztens és alkalmazáskonzisztens helyreállítási pontokat, amikor azok a feladatátvételt.
    * Azt javasoljuk, hogy gyűjtse össze a fizikai kiszolgálók együtt, hogy látni lehessen a munkaterhelések. Több virtuális Gépre kiterjedő konzisztencia engedélyezése befolyásolhatja a teljesítményt a munkaterhelés, és csak akkor használja, ha a gépek is működnek hello azonos munkaterhelés, és konzisztenciára van szükség.

13. Kattintson a **engedélyezze a replikálást**. Hello állapotának nyomon követheti **Védelemengedélyezési** feladat **beállítások** > **feladatok** > **Site Recovery-feladatok**. Hello után **Védelemvéglegesítési** feladat futtatása hello gép készen áll a feladatátvételre.

## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[11. lépés: feladatátvételi teszt futtatása](physical-walkthrough-test-failover.md)
