---
title: "hello forrás és cél Hyper-V replikáció tooAzure (a System Center VMM) az Azure Site Recovery aaaSet |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket tooset forrás és cél Hyper-V virtuális gépek replikálását a VMM felhők tooAzure tárolás az Azure Site Recovery beállításokat"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5edb6d87-25a5-40fe-b6f1-ddf7b55a6b31
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/25/2017
ms.author: raynew
ms.openlocfilehash: 3f8c5386cb64527c775aef636980bac098ee9905
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-hyper-v-with-vmm-replication-tooazure"></a>8. lépés: Hello forrása és célja a Hyper-V (a VMM-mel) replikációs tooAzure beállítása

Miután [létrehozni egy tárolót](vmm-to-azure-walkthrough-create-vault.md) , és adja meg, milyen meg szeretné, hogy tooreplicate, ez a cikk tooconfigure adatforrás és cél beállításait, a helyszíni Hyper-V virtuális gépek a System Center Virtual Machine Manager (VMM) replikálásához felhők tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-hello-source-environment"></a>Hello forráskörnyezet beállítása

S1. Kattintson **Az infrastruktúra előkészítése** > **Forrás** lehetőségre.

    ![Set up source](./media/vmm-to-azure-walkthrough-source-target/set-source1.png)

2. A **forrás előkészítése**, kattintson a **+ VMM** tooadd egy VMM-kiszolgáló.

    ![A forrás beállítása](./media/vmm-to-azure-walkthrough-source-target/set-source2.png)

3. A **kiszolgáló hozzáadása**, ellenőrizze, hogy **System Center VMM-kiszolgáló** megjelenik **kiszolgálótípus** és, hogy hello VMM-kiszolgáló megfelel-e hello [Előfeltételek és URL-címe követelmények](#prerequisites).
4. Töltse le a hello Azure Site Recovery Provider telepítőfájlját.
5. Hello regisztrációs kulcs letöltése. Erre a telepítő futtatása során lesz szükség. hello kulcs a generálásától öt napig esetén érvényes.

    ![A forrás beállítása](./media/vmm-to-azure-walkthrough-source-target/set-source3.png)

## <a name="install-hello-provider-on-hello-vmm-server"></a>Telepítse a szolgáltató hello hello VMM-kiszolgálón

1. Futtassa a szolgáltató telepítőfájl hello hello VMM-kiszolgálón.
2. A **Microsoft Update** lapon kérheti a frissítések beszerzését, így a rendszer a Microsoft Update-szabályzatnak megfelelően telepíteni fogja a Providerhez kiadott frissítéseket.
3. A **telepítési**, fogadja el vagy módosítsa a hello Provider alapértelmezett telepítési helyét, majd kattintson **telepítése**.

    ![Telepítés helye](./media/vmm-to-azure-walkthrough-source-target/provider2.png)
4. Kattintson a telepítés befejezése után **regisztrálása** tooregister hello VMM-kiszolgáló hello tárolóban lévő állapottal.
5. A hello **tároló beállításait** kattintson **Tallózás** tooselect hello tároló kulcsfájlját. Adja meg a hello Azure Site Recovery-előfizetést és hello tároló nevére.

    ![Kiszolgáló regisztrációja](./media/vmm-to-azure-walkthrough-source-target/provider10.png)
6. A **internetkapcsolat**, adja meg, hogyan hello VMM-kiszolgálón futó Provider keresztül fog csatlakozni tooSite helyreállítási hello hello internet.

   * Ha közvetlenül hello szolgáltató tooconnect, jelölje be **csatlakozzon közvetlenül a Site Recovery tooAzure proxy nélkül**.
   * Ha az aktuálisan használt proxy hitelesítést igényel, vagy toouse egyéni proxyt szeretne, válassza ki a **csatlakozzon a Site Recovery proxykiszolgálóval tooAzure**.
   * Ha egyéni proxyt használ, adja meg a hello cím, port és hitelesítő adatait.
   * Ha a proxy használata esetén kell már engedélyezte a hello URL-címeket a [Előfeltételek](#on-premises-prerequisites).
   * Ha egyéni proxyt használ, a VMM RunAs-fiókot (DRAProxyAccount) használatával jön létre automatikusan a megadott hello proxy hitelesítő adatokat. Hello-proxy kiszolgáló konfigurálása, így a fiók elvégezhesse a hitelesítést. hello VMM RunAs-fiók beállításait hello VMM-konzolon módosíthatja. A **beállítások**, bontsa ki a **biztonsági** > **futtató fiókok**, majd módosítsa a draproxyaccount jelszavát hello jelszót. Szüksége lesz toorestart hello VMM szolgáltatást, hogy ez a beállítás lép érvénybe.

     ![internet](./media/vmm-to-azure-walkthrough-source-target/provider13.png)
7. Fogadja el, vagy módosítsa az adatok titkosításához automatikusan létrehozott SSL-tanúsítvány hello helyét. Ez a tanúsítvány akkor használatos, ha engedélyezi az adattitkosítást a hello Azure Site Recovery portálon az Azure által védett felhő. Őrizze biztonságos helyen a tanúsítványt. A feladatátvételi tooAzure futtatásakor szüksége lehet rájuk toodecrypt, ha engedélyezett az adattitkosítás.
8. A **kiszolgálónév**, adjon meg egy rövid nevet tooidentify hello VMM-kiszolgáló hello tárolóban lévő állapottal. Fürtkonfiguráció adja meg a hello VMM-fürtszerepkör nevét.
9. Engedélyezése **felhőmetaadatok szinkronizálása**, ha azt szeretné, toosynchronize metaadatokat VMM-kiszolgálón hello hello tárolóban futó összes felhő. Ez a művelet csak egyszer minden kiszolgálón toohappen van szüksége. Ha nem szeretné, toosynchronize összes felhő, hagyja bejelölve ezt a beállítást, és szinkronizálja egyenként a hello felhőtulajdonságok hello VMM-konzolon. Kattintson a **regisztrálása** toocomplete hello folyamat.

    ![Kiszolgáló regisztrációja](./media/vmm-to-azure-walkthrough-source-target/provider16.png)
10. Elindul a regisztráció. Regisztráció befejezése után hello kiszolgáló megjelenik **Site Recovery-infrastruktúra** > **VMM-kiszolgáló**.


## <a name="install-hello-azure-recovery-services-agent-on-hyper-v-hosts"></a>Hello Azure Recovery Services agent telepítése a Hyper-V gazdagépek

1. Hello szolgáltató beállítása után szüksége toodownload hello telepítőfájl hello Azure Recovery Services Agent ügynököt. Futtassa a telepítőt a VMM-felhő hello minden Hyper-V kiszolgálón.

    ![Hyper-V helyek](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent1.png)
2. Az **Előfeltételek ellenőrzése** részben kattintson a **Tovább** gombra. A rendszer automatikusan telepíti a hiányzó előfeltételeket.

    ![Recovery Services Agent, előfeltételek](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent2.png)
3. A **telepítési beállítások**, fogadja el, vagy módosítsa a hello telepítési helyét, és hello gyorsítótár helyét. Hello gyorsítótár konfigurálhat egy meghajtón, amelyen legalább 5 GB tárterület érhető el, de azt javasoljuk, hogy a gyorsítótárazáshoz használt lemezen legalább 600 GB szabad terület. Ezt követően kattintson a **Telepítés** gombra.
4. Kattintson a telepítés befejezése után **Bezárás** toofinish.

    ![A MARS Agent regisztrálása](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent3.png)

### <a name="command-line-installation"></a>Parancssori telepítés
Hello Microsoft Azure Recovery Services Agent a parancssorból a következő parancs hello segítségével telepítheti:

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-toosite-recovery-from-hyper-v-hosts"></a>Internetes proxy hozzáférés tooSite helyreállítási Hyper-V gazdagépekről beállítása

hello Recovery Services Agent ügynököt a Hyper-V gazdagépeken futó virtuális gép replikációs internet access tooAzure igényeinek. Ha elérni hello internet egy proxyn keresztül, állítsa be az alábbiak szerint:

1. Hello Microsoft Azure Backup szolgáltatás MMC beépülő modul megnyitásához hello Hyper-V gazdagépen. Alapértelmezés szerint a Microsoft Azure Backup parancsikonja áll rendelkezésre, hello asztalon vagy a C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Kattintson a beépülő modul hello **tulajdonságainak módosítása**.
3. A hello **proxykonfigurációt** lapra, adja meg a proxykiszolgáló-adatok.

    ![A MARS Agent regisztrálása](./media/vmm-to-azure-walkthrough-source-target/mars-proxy.png)
4. Ellenőrizze, hogy hello ügynök képes elérni hello URL-címeket a hello [Előfeltételek](#on-premises-prerequisites).

## <a name="set-up-hello-target-environment"></a>Hello célkörnyezet beállítása
Adja meg a replikáláshoz használt hello az Azure storage-fiók toobe, és hello Azure hálózati toowhich Azure virtuális gépek a feladatátvételt követően csatlakozhatnak.

1. Kattintson a **infrastruktúra előkészítése** > **cél**válassza ki a hello előfizetés, ahová átadja a virtuális gépek toocreate hello erőforráscsoport hello. Válassza ki a beállítani kívánt toouse (klasszikus vagy erőforrás-kezelés) Azure virtuális gépek a feladatátvételt hello hello telepítési modell.

    ![Storage](./media/vmm-to-azure-walkthrough-source-target/enablerep3.png)

2. A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.

    ![Storage](./media/vmm-to-azure-walkthrough-source-target/compatible-storage.png)

3. Ha még nem hozott létre egy tárfiókot, és azt szeretné, hogy egy toocreate erőforrás-kezelő használatával kattintson **+ tárfiók** toodo megtenni.  A hello **storage-fiók létrehozása** panelen adja meg egy fiók nevét, típusát, előfizetés és hely. hello fióknak kell lennie a hello hello és ugyanazon a helyen Recovery Services-tároló.

   ![Storage](./media/vmm-to-azure-walkthrough-source-target/gs-createstorage.png)


   * Ha azt szeretné, toocreate hello klasszikus modellt használó tárfiókot, ehhez a hello Azure-portálon. [További információ](../storage/common/storage-create-storage-account.md)
   * A replikált adatok használata a prémium szintű storage-fiók, állítsa be egy további standard szintű tárfiók toostore replikálási naplókhoz, folyamatban lévő változtatások tooon helyszíni adatok rögzítéséhez.
5. Ha még nem hozott létre az Azure-hálózatot, és azt szeretné, hogy egy toocreate erőforrás-kezelő használatával kattintson **+ hálózat** toodo megtenni. A hello **virtuális hálózat létrehozása** panelen adja meg a hálózati név, címtartomány, alhálózati adatokat, előfizetést és helyet. hello hálózat legyen hello hello és ugyanazon a helyen Recovery Services-tároló.

   ![Network (Hálózat)](./media/vmm-to-azure-walkthrough-source-target/gs-createnetwork.png)

   Ha azt szeretné, toocreate hello klasszikus modellt használó hálózatot, ehhez a hello Azure-portálon. [További információk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).





## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[9. lépés: hálózatleképezés konfigurálása](vmm-to-azure-walkthrough-network-mapping.md)
