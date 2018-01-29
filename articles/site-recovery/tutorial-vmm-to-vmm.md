---
title: "Állítsa be a vész-helyreállítási Hyper-V virtuális gépek az Azure Site Recovery a helyszíni helyek közötti |} Microsoft Docs"
description: "Megtudhatja, hogyan beállításához vész-helyreállítási Hyper-V virtuális gépek az Azure Site Recovery a helyszíni helyek közötti."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 65eda71c-3ca3-41bc-b02d-00fecc1557d7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.author: raynew
ms.openlocfilehash: 1647e9d69da3e991bec4e00b3a1083a254fa9550
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/01/2017
---
# <a name="set-up-disaster-recovery-for-hyper-v-vms-to-your-secondary-on-premises-site"></a>Beállítása a vész-helyreállítási Hyper-V virtuális gépek esetén a másodlagos helyszíni helyre

A [Azure Site Recovery](site-recovery-overview.md) szolgáltatás vész-helyreállítási stratégiát infrastruktúrája azzal segíti a kezelése és koordinálása replikációjának, feladatátvételének és feladat-visszavétel a helyszíni gépeket, és az Azure virtuális gépek (VM).

Az oktatóanyag bemutatja, hogyan állíthat be katasztrófa utáni helyreállítás egy másodlagos helyre, a System Center Virtual Machine Manager (VMM) felhőkben felügyelt helyszíni Hyper-V virtuális gépek. Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * A helyszíni VMM-kiszolgáló és a Hyper-V-gazdagépek előkészítése
> * A Site Recovery Recovery Services-tároló létrehozása 
> * Állítsa be a forrás és cél replikációs környezetekben. 
> * A hálózatleképezéseket (Ha a System Center VMM által felügyelt Hyper-V)
> * Replikációs házirend létrehozása
> * A virtuális gépek replikálásának engedélyezése

## <a name="prerequisites"></a>Előfeltételek

Az oktatóanyag elvégzéséhez:

- Tekintse át a [forgatókönyv architektúrája és összetevői](concepts-hyper-v-to-secondary-architecture.md).
- Tekintse át a [igényeinek támogatására](site-recovery-support-matrix-to-sec-site.md) lévő valamennyi összetevőnél.
- Győződjön meg arról, hogy a VMM-kiszolgálók és a Hyper-V-gazdagépek megfelelnek [igényeinek támogatására](site-recovery-support-matrix-to-sec-site.md).
- Ellenőrizze, hogy a replikálni kívánt virtuális gépeket ahhoz [replikált gép támogatási](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions).
- Készüljön elő a hálózatleképezésre VMM-kiszolgálókon.

### <a name="prepare-for-network-mapping"></a>A hálózatleképezés előkészítése

[A hálózatleképezés](site-recovery-network-mapping.md) megfelelteti a helyszíni VMM-Virtuálisgép-hálózatok forrása és célja felhők. Leképezés a következőket teszi:

- Virtuális gépek csatlakozik a megfelelő cél Virtuálisgép-hálózatok a feladatátvételt követően. 
- A cél Hyper-V gazdakiszolgálókra optimális helyezi a replika virtuális gép. 
- Ha nem adja meg a hálózatleképezés, a replika virtuális gépek nem csatlakozik Virtuálisgép-hálózatot a feladatátvételt követően.

Készítse elő a VMM a következőképpen:

1. Győződjön meg arról, hogy [VMM logikai hálózatok](https://docs.microsoft.com/system-center/vmm/network-logical) a forrás és cél VMM-kiszolgálókon.
    - A forráskiszolgálón a logikai hálózatot, amelyben a Hyper-V-gazdagépek találhatók a forrás-felhő társítva kell lennie.
    - A célkiszolgálón a logikai hálózat társítva a célfelhő kell lennie.
1. Győződjön meg arról, hogy [Virtuálisgép-hálózatok](https://docs.microsoft.com/system-center/vmm/network-virtual) a forrás és cél VMM-kiszolgálókon. Virtuálisgép-hálózatot kösse össze az egyes helyeken a logikai hálózathoz.
2. A forrás Hyper-V gazdagépeken a forrás Virtuálisgép-hálózathoz csatlakozzon a virtuális gépek. 


## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a>Válassza ki a védelmi cél

Válassza ki, hogy mit szeretne replikálni, és hova.

1. Kattintson a **Site Recovery** > **1. lépés: az infrastruktúra előkészítése** > **védelmi cél**.
2. Válassza ki **helyreállítási hely**, és válassza ki **Igen, a Hyper-V-vel**.
3. Válassza ki **Igen** annak jelzésére, hogy a VMM segítségével a Hyper-V-gazdagépek kezelése.
4. Válassza ki **Igen** Ha egy másodlagos VMM-kiszolgálóval rendelkezik. Ha egy VMM-kiszolgálón felhők közötti replikáció telepít, kattintson a **nem**. Ezután kattintson az **OK** gombra.


## <a name="set-up-the-source-environment"></a>A forráskörnyezet beállítása

Az Azure Site Recovery Provider telepítése a VMM-kiszolgálókon, és Fedezze fel, és regisztrálja a kiszolgálót a tárolóban.

1. Kattintson **Az infrastruktúra előkészítése** > **Forrás** lehetőségre.
2. A **Forrás előkészítése** ablakban kattintson a **+ VMM** gombra a VMM-kiszolgálók felvételéhez.
3. A **kiszolgáló hozzáadása**, ellenőrizze, hogy **System Center VMM-kiszolgáló** megjelenik **kiszolgálótípus**.
4. Töltse le az Azure Site Recovery Provider telepítőfájlját.
5. Töltse le a regisztrációs kulcsot. Ez szükséges a szolgáltató telepítése során. A kulcs a generálásától számított öt napig érvényes.

    ![A forrás beállítása](./media/tutorial-vmm-to-vmm/source-settings.png)

6. Telepítse a szolgáltató minden VMM-kiszolgálón. Nem kell explicit módon telepít semmit a Hyper-V gazdagépeken.

### <a name="install-the-azure-site-recovery-provider"></a>Az Azure Site Recovery Provider telepítése

1. Futtassa a Providert telepítőfájl minden VMM-kiszolgálón. Ha a VMM fürtben lett telepítve, telepítse először az alábbiak szerint:
    -  Telepítse a szolgáltató aktív csomópontra, és fejezze be a telepítést, a VMM-kiszolgáló regisztrálása a tárolóban lévő állapottal.
    - Ezután telepítse a szolgáltatót a többi csomóponton. Fürtcsomópontok összes futtassa a szolgáltató azonos verziója.
2. A telepítő néhány előfeltétel-ellenőrzéseket futtatja, és a VMM szolgáltatás engedélyt kér. A VMM szolgáltatás automatikusan újraindul a telepítő befejezése után. Ha telepíti a VMM-fürthöz, megkéri a fürtszerepkör leállítása.
3. A **Microsoft Update**, lapon kérheti a adja meg, hogy szolgáltató frissítések telepítve vannak-e a Microsoft Update-szabályzatnak megfelelően.
4. A **telepítési**, fogadja el vagy módosítsa az alapértelmezett telepítési hely, és kattintson a **telepítése**.
5. Kattintson a telepítés befejezése után **regisztrálása** regisztrálni a kiszolgálót a tárolóban lévő állapottal.

    ![Telepítés helye](./media/tutorial-vmm-to-vmm/provider-register.png)
6. A **Tároló neve** résznél ellenőrizze a tároló nevét, amelyben a kiszolgálót regisztrálni fogja. Kattintson a **Tovább** gombra.
7. A **proxykapcsolatot**, adja meg, hogy a VMM-kiszolgálón futó Provider hogyan csatlakozzon az Azure-bA.
   - Megadhatja, hogy a szolgáltató közvetlenül az internethez, vagy egy proxyn keresztül kell-e csatlakozni. Adja meg a proxybeállítások helyességét, igény szerint.
   - Ha proxyt használ, a VMM RunAs-fiókot (DRAProxyAccount) automatikusan létrejön, a megadott hitelesítő adatokat használ. Állítsa be úgy a proxykiszolgálót, hogy ez a fiók elvégezhesse a hitelesítést. A RunAs fiók beállításait módosíthatja a VMM-konzol > **beállítások** > **biztonsági** > **futtató fiókok**.
   - Indítsa újra a VMM szolgáltatást módosításainak frissítésére.
8. A **regisztrációs kulcs**, válassza ki a letöltött és a VMM-kiszolgálóra másolt kulcsot.
9. A titkosítási beállítás nem releváns ebben a forgatókönyvben. 
10. A **Kiszolgáló neve** mezőben adjon meg egy, a tárolóban regisztrált VMM-kiszolgálót azonosító rövid nevet. A fürt adja meg a VMM-fürtszerepkör nevét.
11. A **Synchronize cloud metaadatok**, adja meg, hogy a VMM-kiszolgálón futó összes felhő metaadatait szinkronizálni szeretné. Ezt a műveletet kiszolgálónként csak egyszer szükséges elvégezni. Ha nem kívánja az összes felhő szinkronizálásához, hagyja bejelölve ezt a beállítást. Minden felhő egyenként, szinkronizálhatja a VMM-konzolon a felhők tulajdonságainál.
12. A folyamat befejezéséhez kattintson a **Next** (Tovább) gombra. A regisztrációt követően a Site Recovery lekéri a VMM-kiszolgálóról metaadatait. A kiszolgáló megjelenik **kiszolgálók** > **VMM-kiszolgálókon** a tárolóban lévő állapottal.
13. Miután a tárolóban, megjelenik a kiszolgáló **forrás** > **forrás előkészítése** válassza ki a VMM-kiszolgálót, és válassza ki a felhőt, amelyben a Hyper-V gazdagépen helyezkedik el. Ezután kattintson az **OK** gombra.


## <a name="set-up-the-target-environment"></a>A célkörnyezet beállítása

Válassza ki a cél VMM-kiszolgáló és a felhő:

1. Kattintson a **infrastruktúra előkészítése** > **cél**, és válassza ki a cél VMM-kiszolgálóval.
2. Szinkronizálva legyenek a Site Recovery VMM-felhő jelennek meg. Válassza ki a megcélzott felhőt.

   ![cél](./media/tutorial-vmm-to-vmm/target-vmm.png)


## <a name="set-up-a-replication-policy"></a>A replikációs házirend beállítása

Mielőtt elkezdené, győződjön meg arról, hogy minden gazdagép a házirend ugyanazt az operációs rendszert. Ha a gazdagép Windows Server különböző verzióit futtatják, szüksége több replikációs házirend.

1. Új replikációs szabályzat létrehozásához kattintson az **Infrastruktúra előkészítése** > **Replikációs beállítások** > **+Létrehozás és társítás** elemre.
2. A **Házirend létrehozása és társítása** beállításnál adja meg a szabályzat nevét. A forrás és cél típusnak kell lennie **Hyper-V**.
3. A **Hyper-V gazdagép verziószámánál**, válassza ki, melyik operációs rendszer fut az állomáson.
4. A **hitelesítési típus** és **hitelesítési portot**, adja meg, hogy az elsődleges és a helyreállítási Hyper-V gazdakiszolgálók közötti forgalom hitelesítése.
    - Válassza ki **tanúsítvány** kivéve, ha Kerberos-környezetben működő rendelkezik. Az Azure Site Recovery automatikusan HTTPS-hitelesítési tanúsítványok konfigurálja. Nem kell manuálisan semmit.
    - Alapértelmezés szerint a Windows tűzfalat a Hyper-V gazdakiszolgálók port 8083 és 8084 (a tanúsítványok) fognak megnyílni.
    - Ha **Kerberos**, a Kerberos jegyet a gazdakiszolgálók a kölcsönös hitelesítéshez használható. A Kerberos egy csak a Windows Server 2012 R2 vagy újabb rendszeren futó Hyper-V gazdakiszolgálók szükséges.
1. A **Másolás gyakorisága** elemmel meghatározhatja, hogy milyen gyakran szeretné replikálni a módosult adatokat a kezdeti replikációt követően (ez lehet 30 másodperc, 5 perc vagy 15 perc).
2. A **helyreállításipont-megőrzést**, adja meg a \how hosszú (óra) a megőrzési időszak lesz az egyes helyreállítási pontok. Replikált gép ablak belüli bármelyik pontra állíthatók helyre.
3. A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg, hogy milyen gyakran (1 – 12 órába) helyreállítási pontokat tartalmazó alkalmazáskonzisztens pillanatképeket jönnek létre. Hyper-V két különböző használja:
    - **Standard pillanatkép**: lefedő növekményes pillanatképet a teljes virtuális gép.
    - **Alkalmazáskonzisztens pillanatkép**: pont időponthoz kötött pillanatképet készít a virtuális Gépen belül az alkalmazásadatok. Kötet árnyékmásolata szolgáltatás (VSS) biztosítja, hogy a alkalmazások konzisztens állapotban legyenek, a pillanatkép készítésének időpontjában. Alkalmazás teljesítménye a forrás virtuális gépek alkalmazáskonzisztens pillanatképeket engedélyezése, hatással van. Állítson be egy, amely kisebb, mint a további helyreállítási pontok száma.
4. A **adatátvitel tömörítésének**, adja meg, hogy az átvitt adatokat kell a tömörített.
5. Válassza ki **replika virtuális gép törlése**adja meg, hogy a replika virtuális gépet törölni kell a forrás virtuális gép védelmének letiltásakor. Ha a forrás virtuális gép el van távolítva a Site Recovery konzolján védelmének letiltásakor engedélyezi ezt a beállítást, a Site Recovery beállításait a VMM el lesznek távolítva a VMM-konzolon, és a replika törlése.
6. A **kezdeti replikációs módszer**, ha a hálózaton keresztül, replikál adja meg, hogy a kezdeti replikáció elindítása, vagy átütemezheti azt. Ha a hálózati sávszélesség, érdemes ütemezni, amikor kevesen használják az kívül. Ezután kattintson az **OK** gombra.

     ![Replikációs szabályzat](./media/tutorial-vmm-to-vmm/replication-policy.png)
     
7. Az új házirend automatikusan a VMM-felhő társítva. A **replikációs házirend**, kattintson a **OK**. 


## <a name="enable-replication"></a>A replikáció engedélyezése

1. Kattintson a **alkalmazás replikálása** > **forrás**. 
2. A **forrás**, válassza ki a VMM-kiszolgáló és a felhőben, amelyben a replikálni kívánt Hyper-V-gazdagépek találhatók. Ezután kattintson az **OK** gombra.
3. A **cél**, ellenőrizze a másodlagos VMM-kiszolgáló és a felhőben.
4. A **virtuális gépek**, válassza ki a listából védeni kívánt virtuális gépeket.


Előrehaladásának nyomon követheti a **Védelemengedélyezési** műveletét **feladatok** > **Site Recovery-feladatok**. Miután a **Védelemvéglegesítési** feladat befejeződik, a kezdeti replikálás befejeződik, és a virtuális gép készen áll a feladatátvételre.

## <a name="next-steps"></a>Következő lépések

[Vészhelyreállítási próba végrehajtása](tutorial-dr-drill-secondary.md)
