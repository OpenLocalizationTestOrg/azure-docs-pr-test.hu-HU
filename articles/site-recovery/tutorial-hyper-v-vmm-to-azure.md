---
title: "A helyszíni Hyper-V virtuális gépek vész-helyreállítási beállítása az Azure-bA az Azure Site Recovery VMM-felhőkben |} Microsoft Docs"
description: "Útmutató a helyszíni Hyper-V virtuális gépek vész-helyreállítási beállítása a System Center VMM-felhőkben az Azure-ba, az Azure Site Recovery szolgáltatással."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/31/2017
ms.author: raynew
ms.openlocfilehash: 5f364c6f8a88ad53c12909f0e9f10afcce5ef8af
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/03/2018
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>Állítsa be a helyszíni Hyper-V virtuális gépek vész-helyreállítási VMM-felhőkben az Azure-bA

A [Azure Site Recovery](site-recovery-overview.md) szolgáltatás vész-helyreállítási stratégiát infrastruktúrája azzal segíti a kezelése és koordinálása replikációjának, feladatátvételének és feladat-visszavétel a helyszíni gépeket, és az Azure virtuális gépek (VM).

Az oktatóanyag bemutatja, hogyan állíthat be az Azure-bA helyszíni Hyper-V virtuális gépek vész-helyreállítási. Az oktatóanyag fontos, hogy a System Center Virtual Machine Manager (VMM) által felügyelt Hyper-V virtuális gépek. Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Válassza ki a forrásként és célként.
> * A forrás replikációs környezet beállítása, beleértve a helyszíni Site Recovery-összetevőkhöz, és a cél replikálási környezetet.
> * A hálózatleképezés, beállítása a VMM Virtuálisgép-hálózatok és az Azure virtuális hálózatok közötti.
> * Replikációs házirend létrehozása
> * A virtuális gépek replikálásának engedélyezése

Ez az egy sorozat harmadik oktatóanyaga. Ez az oktatóanyag feltételezi, hogy már végrehajtotta a feladatokat a az előző oktatóanyagok:

1. [Az Azure előkészítése](tutorial-prepare-azure.md)
2. [A helyszíni Hyper-V előkészítése](tutorial-prepare-on-premises-hyper-v.md)

Mielőtt elkezdené, érdemes [tekintse át a architektúra](concepts-hyper-v-to-azure-architecture.md) a vész-helyreállítási forgatókönyv esetében.



## <a name="select-a-replication-goal"></a>Replikációs cél

1. A **minden szolgáltatás** > **Recovery Services-tárolók**, kattintson a tároló nevére, ezek az oktatóanyagok a használjuk **ContosoVMVault**.
2. A **bevezetés**, kattintson a **Site Recovery**. Kattintson a **infrastruktúra előkészítése**
3. A **védelmi cél** > **hol vannak a található gép**, jelölje be **helyszíni**.
4. A **hol szeretné replikálni a gépeket**, jelölje be **az Azure-bA**.
5. A **a virtuális gép**, jelölje be **Igen, a Hyper-V-vel**.
6. A **System Center VMM használatával**, jelölje be **Igen**. Ezután kattintson az **OK** gombra.

    ![Replikációs cél](./media/tutorial-hyper-v-vmm-to-azure/replication-goal.png)



## <a name="set-up-the-source-environment"></a>A forráskörnyezet beállítása

A forráskörnyezettel beállításakor az Azure Site Recovery Provider és az Azure Recovery Services Agent ügynök telepítése, és regisztrálja a helyszíni kiszolgálók a tárolóban. 

1. A **infrastruktúra előkészítése**, kattintson a **forrás**.
2. A **Forrás előkészítése** ablakban kattintson a **+ VMM** gombra a VMM-kiszolgálók felvételéhez. A **kiszolgáló hozzáadása**, ellenőrizze, hogy **System Center VMM-kiszolgáló** megjelenik **kiszolgálótípus**.
3. Töltse le a Microsoft Azure Site Recovery Provider telepítőjéhez.
4. Töltse le a tárolóregisztrációs kulcsot. Ez szükséges a szolgáltató telepítése futtatásakor. A kulcs a generálásától számított öt napig érvényes.
5. Töltse le a Recovery Services Agent ügynököt.

    ![Letöltés](./media/tutorial-hyper-v-vmm-to-azure/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>A szolgáltató telepítése a VMM-kiszolgálón

1. Az Azure Site Recovery Provider telepítése varázslóban > **Microsoft Update**, részt vevő szolgáltató frissítések keresése a Microsoft Update segítségével.
2. A **telepítési**fogadja el az alapértelmezett telepítési hely a szolgáltató, és kattintson a **telepítése**. 
3. A telepítést követően a Microsoft Azure Site Recovery regisztrációs varázsló > **tároló beállításait**, kattintson **Tallózás**, és a **kulcsfájlja**, jelölje be a tárolóbeli kulccsal fájlt letöltve.
4. Adja meg az Azure Site Recovery-előfizetést, és a tároló neve (**ContosoVMVault**). Adjon meg egy rövid nevet a VMM-kiszolgálón való azonosításának megkönnyítése érdekében a tárolóban lévő állapottal.
5. A **proxybeállítások**, jelölje be **közvetlen kapcsolódás proxy nélkül Azure Site Recovery**.
6. Fogadja el az alapértelmezett helyet, az adatok titkosítására használt tanúsítvány. Amikor a rendszer átadja a titkosított adatok lesz visszafejtve.
7. A **Synchronize cloud metaadatok**, jelölje be **szinkronizálási felhőalapú meta adatokat a Site Recovery portálhoz**. Ezt a műveletet kiszolgálónként csak egyszer szükséges elvégezni. Kattintson a **regisztrálása**.
8. Miután a kiszolgáló regisztrálva van a tárolóban, kattintson **Befejezés**.

Regisztráció befejezése után az Azure Site Recovery lekéri a kiszolgálóról metaadatok, és a VMM-kiszolgáló megjelenik **Site Recovery-infrastruktúra**.

### <a name="install-the-recovery-services-agent"></a>A Recovery Services agent telepítése

Az ügynök telepítése minden replikálni kívánt virtuális gépeket tartalmazó Hyper-V gazdagépen.

1. A Microsoft Azure helyreállítási szolgáltatások ügynök telepítővarázslójának > **szükséges előfeltételek ellenőrzése**, kattintson a **következő**. Automatikusan telepíti a hiányzó előfeltételeket.
2. A **telepítési beállítások**, fogadja el a telepítés helyét, illetve a gyorsítótár helyét. A gyorsítótár-meghajtón kell legalább 5 GB tárhelyet. Azt javasoljuk, hogy olyan meghajtót, amelyen legalább 600 GB szabad terület. Ezt követően kattintson a **Telepítés** gombra.
3. A **telepítési**, a telepítés befejezését követően, kattintson a **Bezárás** a varázsló befejezéséhez.

    ![Ügynök telepítése](./media/tutorial-hyper-v-vmm-to-azure/mars-install.png)
    

## <a name="set-up-the-target-environment"></a>A célkörnyezet beállítása

1. Kattintson a **infrastruktúra előkészítése** > **cél**.
2. Válassza ki az előfizetés és az erőforráscsoport (**ContosoRG**) található, amely az Azure virtuális gépek létrehozza a feladatátvételt követően.
3. Válassza ki a **erőforrás-kezelő "** üzembe helyezési modellben.

A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.


## <a name="configure-network-mapping"></a>Hálózatleképezés konfigurálása

1. A **Site Recovery-infrastruktúra** > **Hálózatleképezések** > **Hálózatleképezés** menüpontban kattintson a **+Hálózatleképezés** ikonra.
2. A **hálózatleképezés hozzáadása**, válassza ki a forrás VMM-kiszolgálón. Válassza ki **Azure** céljaként.
3. Ellenőrizze az előfizetést, illetve a feladatok átadását követően használatos üzembe helyezési modellt.
4. A **Forráshálózat**, válassza ki a forrás helyi Virtuálisgép-hálózatot.
5. A **célhálózat**, válassza ki az Azure-hálózatot, mely replika Azure virtuális gépek kerülnek, ha a feladatátvételt követően jönnek létre. Ezután kattintson az **OK** gombra.

    ![Hálózatleképezés](./media/tutorial-hyper-v-vmm-to-azure/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>A replikációs házirend beállítása

1. Kattintson a **infrastruktúra előkészítése** > **replikációs beállítások** > **+ létrehozás és társítás**.
2. A **házirend létrehozása és társítása**, megadhatja a házirend nevét **ContosoReplicationPolicy**.
3. Hagyja meg az alapértelmezett beállításokat, és kattintson a **OK**.
    - **Másolás gyakorisága** azt jelzi, hogy a különbözeti adatok (utáni kezdeti replikálás) replikálja, ötpercenként.
    - **Helyreállítási pontok megőrzésének ideje** azt jelzi, hogy az egyes helyreállítási pontok megőrzési windows lesz két két óra.
    - **Alkalmazáskonzisztens pillanatkép gyakorisága** azt jelzi, hogy alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontokat létrehozza-e minden órában.
    - **Kezdeti replikáció kezdési ideje**, azt jelzi, hogy a kezdeti replikáció azonnal megkezdődik.
    - **Az Azure-on tárolt adatok titkosítása** -az alapértelmezett **ki** beállítás jelzi, hogy aktívan adatok Azure-ban nincs titkosítva.
4. A házirend létrehozása után kattintson **OK**. Az újonnan létrehozott szabályzatokat a rendszer automatikusan társítja a VMM-felhővel.

## <a name="enable-replication"></a>A replikáció engedélyezése

1. A **alkalmazás replikálása**, kattintson a **forrás**. 
2. A **forrás**, válassza ki a VMM-felhőben. Ezután kattintson az **OK** gombra.
3. A **cél**, Azure ellenőrizze a tároló előfizetés célként, és válassza ki a **erőforrás-kezelő** modell.
4. Válassza ki a **contosovmsacct1910171607** tárfiók, és a **ContosoASRnet** Azure-hálózatot.
5. A **virtuális gépek** > **válasszon**, válassza ki a replikálni kívánt virtuális Gépet. Ezután kattintson az **OK** gombra.

 Előrehaladásának nyomon követheti a **Védelemengedélyezési** műveletét **feladatok** > **Site Recovery-feladatok**. Miután a **Védelemvéglegesítési** feladat befejeződik, a kezdeti replikálás befejeződik, és a virtuális gép készen áll a feladatátvételre.


## <a name="next-steps"></a>További lépések
[Vészhelyreállítási próba végrehajtása](tutorial-dr-drill-azure.md)
