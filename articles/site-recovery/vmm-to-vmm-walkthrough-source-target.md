---
title: "hello forrás és cél Hyper-V replikáció tooa másodlagos hely az Azure Site Recovery aaaSet |} Microsoft Docs"
description: "Leírja, hogyan tooset akár hello forrás és cél mikor toosecondary VMM hely Hyper-V virtuális gépek replikálása az Azure Site Recovery szolgáltatással."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: fa7809f1-7633-425f-b25d-d10d004e8d0b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 451cb4413ca5c09777a7faf512b1c8ea43e695f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-set-up-hello-replication-source-and-target"></a>6. lépés: Állítson be hello replikációs forrása és célja


A Recovery Services létrehozása után a Hyper-V replikáció tooa VMM másodlagos hely, tároló [Azure Site Recovery](site-recovery-overview.md), ez a cikk tooset elhasználja hello forrás és cél replikációs helyét. 

A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="set-up-hello-source-environment"></a>Hello forráskörnyezet beállítása

Hello Azure Site Recovery Provider telepítése a VMM-kiszolgálókon, és Fedezze fel, és regisztrálja a kiszolgálókat hello tárolóban.

1. Kattintson a **1. lépés: infrastruktúra előkészítése** > **forrás**.

    ![A forrás beállítása](./media/vmm-to-vmm-walkthrough-source-target/goals-source.png)
2. A **forrás előkészítése**, kattintson a **+ VMM** tooadd egy VMM-kiszolgáló.

    ![A forrás beállítása](./media/vmm-to-vmm-walkthrough-source-target/set-source1.png)
3. A **kiszolgáló hozzáadása**, ellenőrizze, hogy **System Center VMM-kiszolgáló** megjelenik **kiszolgálótípus** és, hogy hello VMM-kiszolgáló megfelel-e hello [Előfeltételek](#prerequisites).
4. Töltse le a hello Azure Site Recovery Provider telepítőfájlját.
5. Hello regisztrációs kulcs letöltése. Erre a telepítő futtatása során lesz szükség. hello kulcs a generálásától öt napig esetén érvényes.

    ![A forrás beállítása](./media/vmm-to-vmm-walkthrough-source-target/set-source3.png)
6. Hello Azure Site Recovery Provider telepítése hello VMM-kiszolgálón. Nem kell tooexplicitly telepít semmit a Hyper-V gazdakiszolgálókra.


## <a name="install-hello-azure-site-recovery-provider"></a>Hello Azure Site Recovery Provider telepítése

1. Futtassa a szolgáltató telepítőfájl hello minden VMM-kiszolgálón. Ha a VMM fürtben lett telepítve, tegye a hello hello amikor először telepíti a következő:
    -  Hello szolgáltató telepíthető az aktív csomópont, és a Befejezés hello telepítési tooregister hello VMM-kiszolgáló hello tárolóban.
    - Ezután telepítse hello szolgáltató hello más csomópontokra. Fürtcsomópontok fusson összes hello hello szolgáltató azonos verziója.
2. A telepítő néhány előfeltétel-ellenőrzéseket futtatja, és kéri engedély toostop hello a VMM szolgáltatást. a VMM szolgáltatás hello automatikusan újraindul a telepítő befejezése után. Ha telepíti a VMM-fürthöz, Ön felszólító toostop hello fürtszerepkört.
3. A **Microsoft Update**, dönthet úgy is toospecify a frissítéseket a Microsoft Update-szabályzatnak megfelelően van.
4. A **telepítési**, fogadja el vagy módosítsa a hello alapértelmezett telepítési hely, és kattintson **telepítése**.

    ![Telepítés helye](./media/vmm-to-vmm-walkthrough-source-target/provider-location.png)
5. Kattintson a telepítés befejezése után **regisztrálása** tooregister hello server hello tárolóban lévő állapottal.

    ![Telepítés helye](./media/vmm-to-vmm-walkthrough-source-target/provider-register.png)
6. A **tároló neve**, ellenőrizze, melyik hello a kiszolgálót regisztrálni fogja hello tároló hello nevére. Kattintson a *Tovább* gombra.

    ![Kiszolgáló regisztrációja](./media/vmm-to-vmm-walkthrough-source-target/vaultcred.png)
7. A **internetkapcsolat**, adja meg, hogy hello VMM-kiszolgálón futó hello provider hogyan csatlakozzon a tooAzure.

    ![Internetbeállítások](./media/vmm-to-vmm-walkthrough-source-target/proxydetails.png)

   - Megadhatja a hello szolgáltatót közvetlenül toohello csatlakoznia internetes, vagy egy proxyn keresztül.
   - Adja meg a proxybeállításokat, ha szükséges.
   - Ha proxyt használ, a VMM RunAs-fiókot (DRAProxyAccount) jön létre, megadott hello használatával automatikusan proxy hitelesítő adatokat. Hello-proxy kiszolgáló konfigurálása, így a fiók elvégezhesse a hitelesítést. hello RunAs-fiók beállításait módosíthatja hello VMM-konzol > **beállítások** > **biztonsági** > **futtató fiókok**. Indítsa újra a hello a VMM szolgáltatás tooupdate módosításait.
8. A **regisztrációs kulcs**, jelölje be az Azure Site Recoveryből letöltött, és másolja a toohello VMM-kiszolgáló hello kulcs.
9. hello titkosítási beállítást csak akkor használja, ha a Hyper-V virtuális gépek a VMM-felhők tooAzure replikál. Ha tooa másodlagos helyre replikál nem használható.
10. A **kiszolgálónév**, adjon meg egy rövid nevet tooidentify hello VMM-kiszolgáló hello tárolóban lévő állapottal. Adja meg a fürt konfigurációjába hello VMM-fürtszerepkör nevét.
11. A **Synchronize cloud metaadatok**, jelölje be, hogy a VMM-kiszolgálón hello hello tárolóban futó összes felhő toosynchronize metaadatok szeretné-e. Ez a művelet csak egyszer minden kiszolgálón toohappen van szüksége. Ha az összes felhő toosynchronize nem szeretné, hagyja bejelölve ezt a beállítást, és szinkronizálja egyenként a hello felhőtulajdonságok hello VMM-konzolon.
12. Kattintson a **következő** toocomplete hello folyamat. A regisztrációt követően az Azure Site Recovery által lekéri metaadatait hello VMM-kiszolgálóról. hello kiszolgáló megjelenik a hello **VMM-kiszolgálókon** hello lapján **kiszolgálók** lap hello tárolóban lévő állapottal.

    ![Kiszolgáló](./media/vmm-to-vmm-walkthrough-source-target/provider13.png)
13. Miután hello kiszolgáló elérhető hello Site Recovery konzolján, a **forrás** > **forrás előkészítése** hello VMM kiszolgálót válassza ki, melyik hello Hyper-V állomás válassza hello felhő. Ezután kattintson az **OK** gombra.

Hello szolgáltató hello parancssorból is telepíthető:

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-hello-target-environment"></a>Hello célkörnyezet beállítása

Hello cél VMM-kiszolgáló és a felhő kiválasztása:

1. Kattintson a **infrastruktúra előkészítése** > **cél**, és válassza hello cél VMM-kiszolgálónak toouse.
2. Felhők hello kiszolgálón, amely szinkronizálva legyenek a Site Recovery jelenik meg. Válassza ki a hello megcélzott felhőt.

   ![cél](./media/vmm-to-vmm-walkthrough-source-target/target-vmm.png)



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[7. lépés: hálózatleképezés konfigurálása](vmm-to-vmm-walkthrough-network-mapping.md).
