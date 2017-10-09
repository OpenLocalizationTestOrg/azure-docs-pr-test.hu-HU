---
title: "hello forrás és cél Hyper-V replikáció tooAzure (a System Center VMM nélkül) az Azure Site Recovery aaaSet |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket tooset forrás és cél Hyper-V virtuális gépek tooAzure tárolás az Azure Site Recovery replikálás beállításainak mentése"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d2010d85-77fd-4dea-84f3-1c960ed4c63f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 105b90e6ac053d5b842c54a36c460a26d0f5c2ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-hyper-v-replication-tooazure"></a>8. lépés: Hello forrása és célja a Hyper-V replikáció tooAzure beállítása

Ez a cikk ismerteti, hogyan tooconfigure forrás és cél beállításai replikálása esetén a helyszíni Hyper-V virtuális gépek (a System Center VMM nélkül) tooAzure, használatával hello [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-hello-source-environment"></a>Hello forráskörnyezet beállítása

Hello Hyper-V hely beállítása hello Azure Site Recovery Provider és hello Azure Recovery Services Agent ügynök telepítése a Hyper-V gazdagépek és hello hely regisztrálja hello tárolóban.

1. A **infrastruktúra előkészítése**, kattintson a **forrás**. a Hyper-V gazdagépekhez vagy fürtökhöz, tárolója új Hyper-V hely tooadd kattintson **+ Hyper-V hely**.

    ![A forrás beállítása](./media/hyper-v-site-walkthrough-source-target/set-source1.png)
2. A **hozzon létre Hyper-V hely**, adja meg a hello hely nevét. Ezután kattintson az **OK** gombra. Jelölje ki, hello helyen létrehozott, és kattintson a **+ Hyper-V Server** tooadd egy kiszolgáló toohello helyet.

    ![A forrás beállítása](./media/hyper-v-site-walkthrough-source-target/set-source2.png)

3. A **kiszolgáló hozzáadása** > **kiszolgálótípus**, ellenőrizze, hogy **Hyper-V server** jelenik meg.

    - Győződjön meg arról, hogy hello Hyper-V-kiszolgálónak tooadd hello megfelel [Előfeltételek](#on-premises-prerequisites), és képes tooaccess hello van megadott URL-címeket.
    - Töltse le a hello Azure Site Recovery Provider telepítőfájlját. A fájl tooinstall hello szolgáltatót futtatja, és minden Hyper-V gazdagépen a Recovery Services agent hello.

    ![A forrás beállítása](./media/hyper-v-site-walkthrough-source-target/set-source3.png)


## <a name="install-hello-provider-and-agent"></a>Hello szolgáltató és az ügynök telepítése

1. Futtassa a szolgáltató telepítőfájl hello minden állomáson hozzáadott toohello Hyper-V helyet. Hyper-V fürt telepítése, futtassa a telepítőt a fürt minden csomópontján. Telepítése és regisztrálása a Hyper-V fürt minden csomópontján biztosítja a virtuális gépek védelmének biztosításához, akkor is, ha az áttelepítés után csomópontjai között.
2. A **Microsoft Update** lapon kérheti a frissítések beszerzését, így a rendszer a Microsoft Update-szabályzatnak megfelelően telepíteni fogja a Providerhez kiadott frissítéseket.
3. A **telepítési**, fogadja el vagy módosítsa a hello Provider alapértelmezett telepítési helyét, majd kattintson **telepítése**.
4. A **tároló beállításait**, kattintson a **Tallózás** tooselect hello tároló kulcsfájlját letöltött. Adja meg a hello Azure Site Recovery-előfizetést, hello tároló nevére, és hello Hyper-V hely toowhich hello Hyper-V kiszolgáló tartozik.

    ![Kiszolgáló regisztrációja](./media/hyper-v-site-walkthrough-source-target/provider3.png)

5. A **proxybeállítások**, adja meg, hogyan Hyper-V gazdagépeken futó szolgáltató az internetre csatlakozik a Site Recovery tooAzure hello hello internet.

    * Ha azt szeretné, hello szolgáltató tooconnect közvetlenül válassza **csatlakozzon közvetlenül a Site Recovery tooAzure proxy nélkül**.
    * Ha az aktuálisan használt proxy hitelesítést igényel, vagy hello szolgáltatói kapcsolat toouse egyéni proxyt használni szeretne, válassza ki a **csatlakozzon a Site Recovery proxykiszolgálóval tooAzure**.
    * Ha proxyt használ:
        - Hello cím, port és a hitelesítő adatok megadása
        - Győződjön meg arról, hogy hello URL-címeket a hello [Előfeltételek](#prerequisites) hello proxyn keresztül történő használata engedélyezett.

    ![internet](./media/hyper-v-site-walkthrough-source-target/provider7.png)

6. Kattintson a telepítés befejezése után **regisztrálása** tooregister hello server hello tárolóban lévő állapottal.

    ![Telepítés helye](./media/hyper-v-site-walkthrough-source-target/provider2.png)

7. Regisztráció befejezése után a metaadatok hello Hyper-V kiszolgálóról Azure Site Recovery lekéri, és hello kiszolgálóhoz **Site Recovery-infrastruktúra** > **Hyper-V-gazdagépek**.


## <a name="set-up-hello-target-environment"></a>Hello célkörnyezet beállítása

Adja meg a hello Azure storage-fiók replikációhoz, és hello Azure hálózati toowhich Azure virtuális gépek a feladatátvételt követően csatlakozhatnak.

1. Kattintson a **infrastruktúra előkészítése** > **cél**.
2. Válassza ki a hello előfizetés és hello erőforráscsoportot használni kívánt toocreate hello Azure virtuális gépek a feladatátvételt követően. Válassza ki a hello telepítési modell, amelyet az Azure (klasszikus vagy erőforrás-kezelés) toouse hello virtuális gépek számára.

3. A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.

    - Ha a tárfiók nincs, kattintson a **+ tárolás** toocreate egy erőforrás-kezelő fiók beágyazott. 
    - Ha nem rendelkezik Azure-hálózatot, kattintson a **+ hálózat** toocreate a Resource Manager-alapú hálózati beágyazott.






## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[9. lépés: a replikációs házirend beállítása](hyper-v-site-walkthrough-replication.md)
