---
title: "aaaReplicate Hyper-V virtuális gépek tooa másodlagos helyet az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Ismerteti, hogyan VMM felhők tooa másodlagos VMM webhely használja a Hyper-V virtuális gépek tooreplicate hello Azure-portálon."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: b33a1922-aed6-4916-9209-0e257620fded
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: e79dbdeab74266e843e5146298dc5aadfe66b5df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site-using-hello-azure-portal"></a>Hyper-V virtuális gépek replikálása a VMM felhők tooa VMM másodlagos hely hello Azure-portál használatával
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Klasszikus portál](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Ez a cikk ismerteti, hogyan tooreplicate a helyszíni System Center Virtual Machine Manager (VMM) felhőkben felügyelt Hyper-V virtuális gépek tooa használja a másodlagos hely [Azure Site Recovery](site-recovery-overview.md) a hello Azure-portálon. További tudnivalók ezzel [kialakítandó architektúra](site-recovery-components.md).

A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites"></a>Előfeltételek

**Előfeltétel** | **Részletek**
--- | ---
**Azure** | Szüksége van egy [Microsoft Azure](http://azure.microsoft.com/)-fiókra. Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is. [További információk](https://azure.microsoft.com/pricing/details/site-recovery/) a Site Recovery díjszabásáról.
**A helyszíni VMM** | Javasoljuk, hogy a két VMM-kiszolgáló, egy hello elsődleges hely és másodlagos hello egyet-egyet.<br/><br/> Egyetlen VMM-kiszolgálón felhők közötti replikálhatja.<br/><br/> VMM-kiszolgálókon legalább futnia kell a System Center 2012 SP1 hello legújabb frissítéseit.<br/><br/> VMM-kiszolgáló internetes hozzáférésre van szükségük.
**VMM-felhő** | Minden VMM-kiszolgálót rendelkeznie kell egy vagy több felhőt, és az összes felhő rendelkeznie kell beállított hello Hyper-V kapacitás profil. <br/><br/>Felhő legalább egy VMM-gazdagépcsoportot kell tartalmaznia.<br/><br/> Csak egy VMM-kiszolgálóval rendelkezik, hogy szükséges-e legalább két felhők, tooact elsődleges és másodlagos.
**Hyper-V** | Hyper-V kiszolgálók legalább futnia kell hello Hyper-V szerepkör és a Windows Server 2012 és a legújabb frissítések telepítve rendelkezik hello.<br/><br/> Minden Hyper-V kiszolgáló tartalmazzon legalább egy virtuális gépet.<br/><br/>  Hyper-V gazdakiszolgálók gazdagépcsoportok hello elsődleges és másodlagos VMM-felhőkben kell elhelyezkedniük.<br/><br/> Ha a Hyper-V fürt, a Windows Server 2012 R2 rendszeren futtatja, telepítse [2961977 frissítése](https://support.microsoft.com/kb/2961977)<br/><br/> Ha a Hyper-V fürt, a Windows Server 2012 rendszerben futtatja, fürtszervező nem hozza létre automatikusan Ha egy statikus IP cím alapú fürtöt. Hello fürtszervező kézi konfigurálását. [További információk](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).<br/><br/> Hyper-V kiszolgálók internet-hozzáférésre van szükségük.
**URL-címek** | VMM-kiszolgáló és a Hyper-V-gazdagépek tooreach képesnek kell lennie az URL-címek:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]

## <a name="deployment-steps"></a>A központi telepítés lépései

Ez mit:

1. Előfeltételek ellenőrzése.
2. Hello VMM-kiszolgáló és a Hyper-V-gazdagépek előkészítése.
3. Recovery Services-tároló létrehozása. hello tároló konfigurációs beállításait tartalmazza, és koordinálja a replikációt.
4. Adja meg a forrás, a cél és a replikációs beállításokat.
5. Hello mobilitási szolgáltatást a virtuális gépek telepítése kívánt tooreplicate.
6. Készítse elő a replikáció, és engedélyezze a Hyper-V virtuális gépek replikációját.
7. Futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik.

## <a name="prepare-vmm-servers-and-hyper-v-hosts"></a>Készítse elő a VMM-kiszolgáló és a Hyper-V-gazdagépek

központi telepítés tooprepare:

1. Győződjön meg arról, hogy hello VMM-kiszolgáló és a Hyper-V-gazdagépek megfelelnek a fent leírt hello Előfeltételek, és el lehet érni hello szükséges URL-címeket.
2. A VMM-hálózatokat állítsa be úgy, hogy az [hálózatleképezés](#network-mapping-overview).

    - Győződjön meg arról, hogy hello forrás Hyper-V gazdakiszolgálón futó virtuális gépek csatlakoztatott tooa VMM Virtuálisgép-hálózatot. Erre a hálózatra kell csatolt tooa hello felhő társított logikai hálózat.
    Győződjön meg arról, hogy hello helyreállításhoz használható másodlagos felhő rendelkezik-e a megfelelő Virtuálisgép-hálózatot. A Virtuálisgép-hálózatnak hello másodlagos felhőhöz társított logikai hálózati csatolt tooa kell lennie.

3. Készítse elő a egy [kiszolgálótelepítést egyetlen](#single-vmm-server-deployment), ha azt szeretné, hogy tooreplicate virtuális gépek hello a felhők közötti megegyező VMM-kiszolgálóhoz.

## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson az **Új** > **Felügyelet** > **Recovery Services** elemre.
3. A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban. Ha egynél több előfizetéssel rendelkezik, válasszon egyet ezek közül.
4. [Hozzon létre egy erőforráscsoportot](../azure-resource-manager/resource-group-template-deploy-portal.md), vagy válasszon ki egy meglévőt. Válassza ki a kívánt Azure-régiót. Gép replikált toothis régióban. támogatott toocheck régiók, tekintse meg a cikknek a földrajzi elérhetőséggel [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/)
5. Ha azt szeretné, hogy tooquickly hozzáférés hello tároló a hello irányítópult, kattintson a **PIN-kód toodashboard** > **tároló létrehozása**.

    ![Új tároló](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

hello új tároló megjelenik hello **irányítópult**, a **összes erőforrás**, és a fő hello **Recovery Services-tárolók** panelen.


## <a name="choose-a-protection-goal"></a>Válassza ki a védelmi cél

Válassza ki a kívánt tooreplicate és a tooreplicate, ahová.

2. Kattintson a **Site Recovery** > **1. lépés: az infrastruktúra előkészítése** > **védelmi cél**.
3. Válassza ki **toorecovery hely**, és válassza ki **Igen, a Hyper-V-vel**.
4. Válassza ki **Igen** tooindicate VMM toomanage hello Hyper-V-gazdagépek használata.
5. Válassza ki **Igen** Ha egy másodlagos VMM-kiszolgálóval rendelkezik. Ha egy VMM-kiszolgálón felhők közötti replikáció telepít, kattintson a **nem**. Ezután kattintson az **OK** gombra.

    ![Célok megválasztása](./media/site-recovery-vmm-to-vmm/choose-goals.png)

## <a name="set-up-hello-source-environment"></a>Hello forráskörnyezet beállítása

Hello Azure Site Recovery Provider telepítése a VMM-kiszolgálókon, és Fedezze fel, és regisztrálja a kiszolgálókat hello tárolóban.

1. Kattintson a **1. lépés: infrastruktúra előkészítése** > **forrás**.

    ![A forrás beállítása](./media/site-recovery-vmm-to-vmm/goals-source.png)
2. A **forrás előkészítése**, kattintson a **+ VMM** tooadd egy VMM-kiszolgáló.

    ![A forrás beállítása](./media/site-recovery-vmm-to-vmm/set-source1.png)
3. A **kiszolgáló hozzáadása**, ellenőrizze, hogy **System Center VMM-kiszolgáló** megjelenik **kiszolgálótípus** és, hogy hello VMM-kiszolgáló megfelel-e hello [Előfeltételek](#prerequisites).
4. Töltse le a hello Azure Site Recovery Provider telepítőfájlját.
5. Hello regisztrációs kulcs letöltése. Erre a telepítő futtatása során lesz szükség. hello kulcs a generálásától öt napig esetén érvényes.

    ![A forrás beállítása](./media/site-recovery-vmm-to-vmm/set-source3.png)
6. Hello Azure Site Recovery Provider telepítése hello VMM-kiszolgálón. Nem kell tooexplicitly telepít semmit a Hyper-V gazdakiszolgálókra.


### <a name="install-hello-azure-site-recovery-provider"></a>Hello Azure Site Recovery Provider telepítése

1. Futtassa a szolgáltató telepítőfájl hello minden VMM-kiszolgálón. Ha a VMM fürtben lett telepítve, tegye a hello hello amikor először telepíti a következő:
    -  Hello szolgáltató telepíthető az aktív csomópont, és a Befejezés hello telepítési tooregister hello VMM-kiszolgáló hello tárolóban.
    - Ezután telepítse hello szolgáltató hello más csomópontokra. Fürtcsomópontok fusson összes hello hello szolgáltató azonos verziója.
2. A telepítő néhány előfeltétel-ellenőrzéseket futtatja, és kéri engedély toostop hello a VMM szolgáltatást. a VMM szolgáltatás hello automatikusan újraindul a telepítő befejezése után. Ha telepíti a VMM-fürthöz, Ön felszólító toostop hello fürtszerepkört.
3. A **Microsoft Update**, dönthet úgy is toospecify a frissítéseket a Microsoft Update-szabályzatnak megfelelően van.
4. A **telepítési**, fogadja el vagy módosítsa a hello alapértelmezett telepítési hely, és kattintson **telepítése**.

    ![Telepítés helye](./media/site-recovery-vmm-to-vmm/provider-location.png)
5. Kattintson a telepítés befejezése után **regisztrálása** tooregister hello server hello tárolóban lévő állapottal.

    ![Telepítés helye](./media/site-recovery-vmm-to-vmm/provider-register.png)
6. A **tároló neve**, ellenőrizze, melyik hello a kiszolgálót regisztrálni fogja hello tároló hello nevére. Kattintson a *Tovább* gombra.

    ![Kiszolgáló regisztrációja](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)
7. A **internetkapcsolat**, adja meg, hogy hello VMM-kiszolgálón futó hello provider hogyan csatlakozzon a tooAzure.

    ![Internetbeállítások](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

   - Megadhatja a hello szolgáltatót közvetlenül toohello csatlakoznia internetes, vagy egy proxyn keresztül.
   - Adja meg a proxybeállításokat, ha szükséges.
   - Ha proxyt használ, a VMM RunAs-fiókot (DRAProxyAccount) jön létre, megadott hello használatával automatikusan proxy hitelesítő adatokat. Hello-proxy kiszolgáló konfigurálása, így a fiók elvégezhesse a hitelesítést. hello RunAs-fiók beállításait módosíthatja hello VMM-konzol > **beállítások** > **biztonsági** > **futtató fiókok**. Indítsa újra a hello a VMM szolgáltatás tooupdate módosításait.
8. A **regisztrációs kulcs**, jelölje be az Azure Site Recoveryből letöltött, és másolja a toohello VMM-kiszolgáló hello kulcs.
9. hello titkosítási beállítást csak akkor használja, ha a Hyper-V virtuális gépek a VMM-felhők tooAzure replikál. Ha tooa másodlagos helyre replikál nem használható.
10. A **kiszolgálónév**, adjon meg egy rövid nevet tooidentify hello VMM-kiszolgáló hello tárolóban lévő állapottal. Adja meg a fürt konfigurációjába hello VMM-fürtszerepkör nevét.
11. A **Synchronize cloud metaadatok**, jelölje be, hogy a VMM-kiszolgálón hello hello tárolóban futó összes felhő toosynchronize metaadatok szeretné-e. Ez a művelet csak egyszer minden kiszolgálón toohappen van szüksége. Ha az összes felhő toosynchronize nem szeretné, hagyja bejelölve ezt a beállítást, és szinkronizálja egyenként a hello felhőtulajdonságok hello VMM-konzolon.
12. Kattintson a **következő** toocomplete hello folyamat. A regisztrációt követően az Azure Site Recovery által lekéri metaadatait hello VMM-kiszolgálóról. hello kiszolgáló megjelenik a hello **VMM-kiszolgálókon** hello lapján **kiszolgálók** lap hello tárolóban lévő állapottal.

    ![Kiszolgáló](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)
13. Miután hello kiszolgáló elérhető hello Site Recovery konzolján, a **forrás** > **forrás előkészítése** hello VMM kiszolgálót válassza ki, melyik hello Hyper-V állomás válassza hello felhő. Ezután kattintson az **OK** gombra.

Hello szolgáltató hello parancssorból is telepíthető:

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-hello-target-environment"></a>Hello célkörnyezet beállítása

Válassza ki a hello cél VMM-kiszolgáló és a felhő.

1. Kattintson a **infrastruktúra előkészítése** > **cél**, és válassza hello cél VMM-kiszolgálónak toouse.
2. Felhők hello kiszolgálón, amely szinkronizálva legyenek a Site Recovery jelenik meg. Válassza ki a hello megcélzott felhőt.

   ![cél](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="set-up-replication-settings"></a>Replikációs beállítások megadása

- A replikációs házirend létrehozásakor kell rendelkeznie hello ugyanazon operációs rendszer hello házirendet használó minden gazdagépen. VMM-felhő hello csak Windows Server különböző verzióit futtató Hyper-V-gazdagépek, de ebben az esetben szüksége több replikációs házirend.
- Hello kezdeti replikálás offline végezheti el. [További információ](#prepare-for-initial-offline-replication)

1. Kattintson egy új replikációs házirendet toocreate **infrastruktúra előkészítése** > **replikációs beállítások** > **+ létrehozás és társítás**.

    ![Network (Hálózat)](./media/site-recovery-vmm-to-vmm/gs-replication.png)
2. A **Házirend létrehozása és társítása** beállításnál adja meg a szabályzat nevét. hello forrás és cél-típusnak kell lennie **Hyper-V**.
3. A **Hyper-V gazdagép verziószámánál**, válassza ki, melyik operációs rendszer fut hello gazdagépen.
4. A **hitelesítési típus** és **hitelesítési portot**, adja meg, hogyan hello elsődleges és a helyreállítási Hyper-V gazdakiszolgálók közötti forgalom hitelesítése. Válassza ki **tanúsítvány** kivéve, ha Kerberos-környezetben működő rendelkezik. Az Azure Site Recovery automatikusan HTTPS-hitelesítési tanúsítványok konfigurálja. Nincs szükség a toodo semmit manuálisan. Alapértelmezés szerint a Windows tűzfal hello a Hyper-V gazdakiszolgálókra hello port 8083 és 8084 (a tanúsítványok) fognak megnyílni. Ha **Kerberos**, a Kerberos jegy hello kiszolgálók a kölcsönös hitelesítéshez használható. Vegye figyelembe, hogy ez a beállítás csak a Windows Server 2012 R2 rendszeren futó Hyper-V gazdakiszolgálók szükséges.
5. A **másolás gyakorisága**, adja meg, milyen gyakran tooreplicate különbözeti adatokat hello kezdeti replikálás (30 másodperces, 5 vagy 15 perc) után.
6. A **helyreállításipont-megőrzést**, adja meg, órákban mennyi ideig hello adatmegőrzési időtartam fogja az egyes helyreállítási pontok lehet. Védett gépek lehet helyreállítani egy időszakban tooany pont.
7. A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg, hogy milyen gyakran (1 – 12 órába) helyreállítási pontokat tartalmazó alkalmazáskonzisztens pillanatképeket jönnek létre. Hyper-V két különböző használ – standard pillanatkép, amely lefedő növekményes pillanatképet hello teljes virtuális gép, és egy alkalmazáskonzisztens pillanatkép, amely időpontban pillanatképet készít a hello alkalmazásadatok hello virtuális gépen belül. Alkalmazáskonzisztens pillanatképek a kötet árnyékmásolata szolgáltatás (VSS) tooensure, amely az alkalmazások konzisztens állapotban legyenek, hello pillanatkép készítésének időpontjában. Ha engedélyezi az alkalmazáskonzisztens pillanatképeket, az hatással hello forrás virtuális gépeken futó alkalmazások teljesítményére. Győződjön meg arról, hogy hello beállított érték kisebb, mint hello további helyreállítási pontok száma.
8. A **adatátvitel tömörítésének**, adja meg, hogy van-e a replikált adatok tömörített.
9. Válassza ki **replika virtuális gép törlése**, toospecify, amely a replika virtuális gép hello hello forrás virtuális gép védelmének letiltásakor törölni kell. Hello forrás hello Site Recovery konzolján lekerül virtuális gép védelmének letiltásakor engedélyezi ezt a beállítást, ha a Site Recovery beállításait a VMM hello el lesznek távolítva hello VMM-konzol, és hello replika törlődik.
10. A **kezdeti replikációs módszer**, ha replikál hello hálózaton, adja meg, hogy toostart hello a kezdeti replikálás, vagy átütemezheti azt. toosave hálózati sávszélesség, érdemes lehet tooschedule azt a foglalt munkaidőn kívül. Ezután kattintson az **OK** gombra.

     ![Replikációs szabályzat](./media/site-recovery-vmm-to-vmm/gs-replication2.png)
11. Ha egy új házirendet hoz létre, automatikusan rendelkezik hello VMM-felhő társítva. A **replikációs házirend**, kattintson a **OK**. További VMM-Felhőkben (és a bennük foglalt virtuális gépek hello) társíthatja a replikációs házirendet a **replikációs** > szabályzat neve > **VMM-felhő társítása**.

     ![Replikációs szabályzat](./media/site-recovery-vmm-to-vmm/policy-associate.png)


### <a name="configure-network-mapping"></a>Hálózatleképezés konfigurálása

- További tudnivalók [hálózatleképezés](#prepare-for-network-mapping) megkezdése előtt.
- Ellenőrizze, hogy a virtuális gépek VMM-kiszolgálókon csatlakoztatott tooa Virtuálisgép-hálózatot.


1. A **Site Recovery-infrastruktúra** > **Hálózatleképezés** > **Hálózatleképezések**, kattintson a **+ Hálózatleképezés**.

    ![Hálózatleképezés](./media/site-recovery-vmm-to-azure/network-mapping1.png)
2. A **hálózatleképezés hozzáadása** lapon válassza ki a hello forrás és cél VMM-kiszolgálók. VMM-kiszolgálókon hello társított Virtuálisgép-hálózatok hello beolvasása.
3. A **Forráshálózat**, jelölje be azt szeretné, hogy a VMM-kiszolgáló hello társított Virtuálisgép-hálózatok listája hello toouse hello hálózati.
4. A **célhálózat**, jelölje be azt szeretné, hogy a VMM-kiszolgálón hello másodlagos toouse hello hálózati. Ezután kattintson az **OK** gombra.

    ![Hálózatleképezés](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Ez történik a hálózatleképezés megkezdésekor:

* Minden meglévő replika virtuális gépeket, amelyek megfelelnek a toohello forrás Virtuálisgép-hálózathoz csatlakoztatott toohello cél Virtuálisgép-hálózat lesz.
* Új virtuális gépek, amelyek a forrás Virtuálisgép-hálózathoz csatlakoztatott toohello replikáció után lesz csatlakoztatott toohello cél csatlakoztatott hálózati.
* Ha módosít egy meglévő hozzárendelést egy új hálózati, a replika virtuális gépek csatlakoznak hello új beállításokkal.
* Ha hello célhálózatban már több alhálózat működik, és ezek egyikének rendelkezik hello neve megegyezik a virtual machine hello forrás alhálózati található, akkor hello replika virtuális gép lesz a cél alhálózathoz csatlakoztatott toothat feladatátvételt követően. Ha nincsenek egyező nevű cél alhálózathoz, hello virtuális gép lesz a csatlakoztatott toohello hello hálózat első alhálózatát.

### <a name="configure-storage-mapping"></a>Tárolási leképezés konfigurálása.

[Tároló leképezési](#prepare-for-storage-mapping) hello új Azure-portálon nem támogatott. Azonban engedélyezni lehet Powershell használatával. [További információk](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-7-configure-storage-mapping).

## <a name="step-5-capacity-planning"></a>5. lépés: Kapacitástervezés

Most, hogy az alapszintű infrastruktúra készen áll, fogjon hozzá a kapacitás megtervezéséhez, és döntse el, hogy szüksége van-e további erőforrásokra.

- Töltse le és futtassa a hello [Azure Site Recovery Capacity planner](site-recovery-capacity-planner.md), toogather információt a replikálási környezetre, beleértve a virtuális gépek, virtuális gép, valamint a lemezenkénti lemezterület.
- Valós idejű replikálási adatok begyűjtését követően hello NetQos házirend toocontrol replikáció sávszélesség módosíthatja a virtuális gépek esetén. Tudjon meg többet az [Hyper-V replika forgalom szabályozása](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/), Thomas Maurer blogjában. További információ a hello [New-NetQosPolicy parancsmag](https://technet.microsoft.com/library/hh967468.aspx.).

## <a name="enable-replication"></a>A replikáció engedélyezése

1. Kattintson **2. lépés: Az alkalmazás replikálása** > **Forrás** elemre. Első alkalommal hello replikáció engedélyezése után kattintson **+ replikálás** a hello tároló tooenable a további gépek replikációjának.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-vmm/enable-replication1.png)
2. A **forrás**, válassza ki a VMM-kiszolgáló hello és hello felhő, mely hello tooreplicate kívánt Hyper-V-gazdagépek találhatók. Ezután kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-vmm/enable-replication2.png)
3. A **cél**, ellenőrizze a hello másodlagos VMM-kiszolgáló és a felhő.
4. A **virtuális gépek**, válassza ki a kívánt hello listából tooprotect hello virtuális gépeket.

    ![A virtuális gép védelmének engedélyezése](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

Hello állapotának nyomon követheti **Védelemengedélyezési** műveletét **feladatok** > **Site Recovery-feladatok**. Hello után **Védelemvéglegesítési** feladat befejeződik, hello virtuális gép készen áll a feladatátvételre.

Vegye figyelembe:

- Hello VMM-konzol virtuális gépek védelmét is engedélyezheti. Kattintson a **Védelemengedélyezési** hello eszköztár hello virtuális gép tulajdonságai > **Azure Site Recovery** lapon.
- Replikáció engedélyezése után megtekintheti a hello virtuális gép tulajdonságait a **replikált elemek**. A hello **Essentials** az irányítópult hello replikációs házirend hello virtuális gép és annak állapotát kapcsolatos információkat tekintheti meg. Kattintson a **tulajdonságok** további részleteket.

### <a name="onboard-existing-virtual-machines"></a>A bevezetni meglévő virtuális gépek
Ha meglévő virtuális gépek a VMM Alkalmazásban – a Hyper-V replika replikálja, hogy discoveryt őket az Azure Site Recovery replikációs az alábbiak szerint:

1. Győződjön meg arról, hogy hello hello elsődleges felhőben található meglévő Virtuálisgép üzemeltetési hello Hyper-V kiszolgálón, és hello replika virtuális gépet szolgáltató hello Hyper-V kiszolgálón hello másodlagos felhőben található.
2. Ellenőrizze, hogy a replikációs házirend hello elsődleges VMM-felhőben van konfigurálva.
3. Engedélyezze a hello elsődleges virtuális gép replikációját. Az Azure Site Recovery és a VMM győződjön meg arról, hogy hello azonos másodpéldány-állomás és a virtuális gép észlel, és Azure Site Recovery szeretné újrafelhasználni újra létrehozni a replikációs hello segítségével megadott beállítások.

## <a name="test-your-deployment"></a>A központi telepítés tesztelése

tootest futtathatja a központi telepítés egy [feladatátvételi teszt](site-recovery-test-failover-vmm-to-vmm.md) egyetlen virtuális géphez, vagy [helyreállítási terv létrehozása](site-recovery-create-recovery-plans.md) , amely egy vagy több virtuális gépet tartalmaz.



## <a name="prepare-for-offline-initial-replication"></a>Offline kezdeti replikálás előkészítése

Az első adatmásolás hello offline replikációt teheti meg. Előkészítheti a következőképpen:

* Hello forráskiszolgálón adjon meg egy elérési helyet, mely hello származó adatok exportálása akkor kerül sor. Rendeljen teljes hozzáférés NTFS és megosztási engedélyek toohello VMM szolgáltatás hello exportálási elérési úton. Hello célkiszolgálón akkor adja meg egy elérési útja, amelyből hello adatok importálása megtörténik. Rendelje hozzá a hello ugyanazokkal az engedélyekkel az importálási útvonalat.
* Ha hello importálása vagy exportálása elérési út megosztása, rendeljen hozzájuk csoporttagságot rendszergazda, a kiemelt felhasználó, a Nyomtatófelelősök vagy a kiszolgáló operátor hello VMM-szolgáltatásfiók hello távoli számítógépen mely megosztott hello található.
* Futtató fiókok tooadd állomások használata, a hello importálása és exportálási útvonalakra, rendelje hozzá az olvasási és írási engedélyek toohello futtató fiókokat a VMM-ben.
* Importálás hello és megosztások exportálása nem kell elhelyezni minden olyan számítógépen, használja a Hyper-V gazdagép-kiszolgálón, mert visszacsatolási konfiguráció nem támogatott a Hyper-v.
* Az Active Directory minden Hyper-V gazdakiszolgálón futó virtuális gépeket tartalmazó szeretné, hogy tooprotect, engedélyezheti és konfigurálhatja a korlátozott delegálás tootrust hello távoli számítógépek mely hello importálási és exportálási útvonalak találhatók, az alábbiak szerint:
  1. Hello tartományvezérlőn nyissa meg a **Active Directory – felhasználók és számítógépek**.
  2. Hello konzol konzolfáján kattintson **tartománynév** > **számítógépek**.
  3. Kattintson a jobb gombbal a Hyper-V hello futtató kiszolgáló neve > **tulajdonságok**.
  4. A hello **delegálás** lapra, majd **a számítógépen csak a delegálás toospecified szolgáltatások**.
  5. Kattintson a **bármely hitelesítési protokoll**.
  6. Kattintson a **hozzáadása** > **felhasználók és számítógépek**.
  7. Hello típusnév hello számítógép, amelyen hello exportálási elérési útja > **OK**. Elérhető szolgáltatások hello listáról hello CTRL billentyűt lenyomva, majd kattintson a **cifs** > **OK**. Ismételje meg a hello hello számítógép nevét, hogy állomások hello importálási elérési útja. Ismételje meg a Hyper-V-gazdagép további kiszolgálókat a megfelelő.



## <a name="prepare-for-network-mapping"></a>A hálózatleképezés előkészítése
Hálózatleképezés kapcsolatot hoz létre hello elsődleges és másodlagos VMM-kiszolgálókon a VMM-Virtuálisgép-hálózatok között:

* Optimális helyezze el a replika virtuális gépek másodlagos Hyper-V-gazdagépek a feladatátvételt követően.
* A replika virtuális gépek tooappropriate Virtuálisgép-hálózatok a feladatátvételt követően csatlakozhatnak.

Vegye figyelembe:
- A hálózatleképezés beállítható, hogy két VMM-kiszolgálókon a Virtuálisgép-hálózatok között, vagy egyetlen VMM-kiszolgálón, ha a két hely által felügyelt hello azonos kiszolgálón.
- Leképezési megfelelően van konfigurálva és replikáció engedélyezett-e, a virtuális gépek hello elsődleges helyen lesz csatlakoztatott tooa hálózati és fogja a replikáját hello célhelyen csatlakoznak tooits hálózati képezi le.
- Hálózatok rendelkezik megfelelően beállítva a VMM-ben a célként megadott Virtuálisgép-hálózat kiválasztásakor hálózatra való leképezés során, ha hello VMM forrás felhők hello forrás Virtuálisgép-hálózatot használó jelenik meg, hello az elérhető célkiszolgálók Virtuálisgép-hálózatok hello cél felhők használt együtt védelem.
- Ha hello célhálózatban már több alhálózat működik, és ezek egyikének ugyanazzal a névvel, mely hello a forrás virtuális gép is található, alhálózati hello majd hello hello a replika virtuális gép lesz a cél alhálózathoz csatlakoztatott toothat feladatátvételt követően. Ha nincsenek egyező nevű cél alhálózathoz, hello virtuális gép lesz a csatlakoztatott toohello hello hálózat első alhálózatát.


Íme egy példa tooillustrate a mechanizmus. A következőkben két helyen található Győr és Chicagói rendelkező szervezeteknél.

| **Hely** | **VMM-kiszolgáló** | **A Virtuálisgép-hálózatok** | **Leképezve** |
| --- | --- | --- | --- |
| New York |A VMM-NewYork |VMNetwork1-NewYork |Csatlakoztatott tooVMNetwork1-Chicagói |
| VMNetwork2-NewYork |Nincsenek leképezve: | | |
| Chicago |A VMM-Chicagói |VMNetwork1-Chicagói |Csatlakoztatott tooVMNetwork1-NewYork |
| VMNetwork2-Chicagói |Nincsenek leképezve: | | |

Az ebben a példában:

* A replika virtuális gép létrehozásakor bármely virtuális géphez, amely csatlakoztatott tooVMNetwork1-NewYork csatlakoztatott tooVMNetwork1-Chicagói lesz.
* Amikor a replika virtuális gép VMNetwork2-NewYork vagy VMNetwork2-Chicagói hoz létre, akkor nem fog csatlakoztatott tooany hálózati.

Ez hogyan VMM-felhő beállítása a szervezet példája és hello hello-felhőkhöz társított logikai hálózat.

### <a name="cloud-protection-settings"></a>Felhő védelmi beállításait
| **Védett felhő** | **Felhő védelme** | **Logikai hálózat (New Yorkban)** |
| --- | --- | --- |
| GoldCloud1 |GoldCloud2 | |
| SilverCloud1 |SilverCloud2 | |
| GoldCloud2 |<p>NA</p><p></p> |<p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicagói</p> |
| SilverCloud2 |<p>NA</p><p></p> |<p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicagói</p> |

### <a name="logical-and-vm-network-settings"></a>Logikai és a virtuális gép hálózati beállításai
| **Hely** | **Logikai hálózat** | **Társított Virtuálisgép-hálózat** |
| --- | --- | --- |
| New York |LogicalNetwork1-NewYork |VMNetwork1-NewYork |
| Chicago |LogicalNetwork1-Chicagói |VMNetwork1-Chicagói |
| LogicalNetwork2Chicago |VMNetwork2-Chicagói | |

### <a name="target-networks"></a>Célhálózatok
Ezek a beállítások alapján hello cél Virtuálisgép-hálózat kiválasztásakor, hello a következő táblázatban látható hello lehetőségeket, amelyek elérhetők lesznek.

| **Kiválasztás** | **Védett felhő** | **Felhő védelme** | **A célhálózat érhető el** |
| --- | --- | --- | --- |
| VMNetwork1-Chicagói |SilverCloud1 |SilverCloud2 |Elérhető |
| GoldCloud1 |GoldCloud2 |Elérhető | |
| VMNetwork2-Chicagói |SilverCloud1 |SilverCloud2 |Nincs |
| GoldCloud1 |GoldCloud2 |Elérhető | |


### <a name="failback"></a>Feladat-visszavétel
Mi történik, a feladat-visszavétel (visszirányú replikálás), hello esetben toosee tegyük fel, hogy VMNetwork1-NewYork-e a csatlakoztatott a tooVMNetwork1-Chicago, a beállítások a következő hello rendelkező.

| **Virtuális gép** | **Csatlakoztatott tooVM hálózati** |
| --- | --- |
| VM1 |VMNetwork1-hálózat |
| Vm2 virtuális gépnek (VM1 replika) |VMNetwork1-Chicagói |

Ezekkel a beállításokkal tekintsük át, mi történik, több lehetséges forgatókönyv szerint.

| **A forgatókönyv** | **Eredménye** |
| --- | --- |
| Nem módosult a feladatátvételt követően VM-2 hello hálózati tulajdonságait. |VM-1 csatlakoztatott toohello Forráshálózat marad. |
| VM-2 hálózati tulajdonságainak a feladatátvételt követően módosulnak, és le van választva. |VM-1 le van választva. |
| VM-2 hálózati tulajdonságainak a feladatátvételt követően módosulnak, és a csatlakoztatott tooVMNetwork2-Chicagói. |Ha nincs leképezve VMNetwork2-Chicago, VM-1 le lesz választva. |
| Hálózati leképezése VMNetwork1-Chicagói módosul. |VM-1 csatlakoztatott toohello most már csatlakoztatott hálózati tooVMNetwork1-Chicagói lesz. |


## <a name="prepare-for-single-server-deployment"></a>Az egykiszolgálós telepítés előkészítése


Egyetlen VMM-kiszolgáló csak akkor hajthatja végre a replikálást virtuális gépek Hyper-V gazdagépek a VMM-felhő hello túl[Azure](site-recovery-vmm-to-azure.md) vagy tooa másodlagos VMM-felhőben. Hello első lehetőség zökkenőmentes felhők közötti replikálása nem ajánlott. Ha szeretné, hogy felhők közötti tooreplicate, replikálhatja egyetlen önálló VMM-kiszolgálóhoz, vagy felhőbe archivált Windows fürtben telepített egyetlen VMM-kiszolgálóhoz

### <a name="standalone-vmm-server"></a>Önálló VMM-kiszolgáló

Ebben a forgatókönyvben hello egyetlen VMM-kiszolgáló telepítése virtuális gépként hello elsődleges helyen, és replikálja a virtuális gép tooa másodlagos helyet a Site Recovery és a Hyper-V replika használata.

1. **Állítsa be a Hyper-V virtuális gép VMM**. Javasoljuk, hogy Ön tartozó hello hello a VMM által használt SQL Server-példány közös elhelyezése azonos virtuális gép. Ezzel időt takaríthat meg, csak egy virtuális gép rendelkezik létrehozott toobe. Ha azt szeretné, hogy az SQL Server távoli példányát toouse, és egy esetleges leálláskor, toorecover annak a példánynak előtt kell a VMM állíthatja helyre.
2. **Győződjön meg arról hello VMM-kiszolgáló van konfigurálva, legalább két felhők**. Egy felhőalapú virtuális gépek szeretné, hogy tooreplicate és más felhőalapú hello hello másodlagos helyet fog szolgálni hello fogja tartalmazni. felhőbe, amely tartalmazza a kívánt tooprotect meg kell felelnie hello virtuális gépek hello [Előfeltételek](#prerequisites).
3. Állítsa be a Site Recovery, ebben a cikkben leírtak szerint. Hozzon létre és regisztrálja hello VMM-kiszolgálót a tárolóban, egy replikációs házirendnek és engedélyezze a replikálást. hello forrás és cél VMM neve azonos lesz hello. Adja meg, hogy a kezdeti replikáció hello hálózaton keresztül történik.
4. A hálózatleképezés beállításához le kell képezni hello hello elsődleges felhő toohello Virtuálisgép-hálózat hello másodlagos felhőre vonatkozó Virtuálisgép-hálózatot.
5. Hello Hyper-V Manager konzolon engedélyezze a Hyper-V replika hello Hyper-V gazdagépen, amely tartalmazza a hello VMM virtuális gép, és engedélyezze a replikálást a virtuális gép hello. Ellenőrizze, hogy a hely helyreállítását követően tooensure, hogy a Hyper-V replika beállításait nem felülbírálják a Site Recovery által védett hello VMM virtuális gép tooclouds nem tölti fel.
6. Használhatja a feladatátvételi helyreállítási tervek létrehozásakor hello forrás megegyező VMM-kiszolgálóhoz, és a cél.
7. Egy teljes leállás feladatátvételt, és a következőképpen helyre:

   1. Nem tervezett feladatátvétel hello Hyper-V Manager konzolon hello másodlagos helyen, toofail keresztül hello elsődleges VMM virtuális gépek toohello másodlagos helyen.
   2. Győződjön meg arról, hogy hello VMM virtuális gép működik-e és fut, és hello tárolóban, futtatható egy nem tervezett feladatátvétel toofail keresztüli hello virtuális gépek elsődleges toosecondary felhőből. Véglegesítse hello feladatátvételi, és válassza ki a másik helyreállítási pontot, ha szükséges.
   3. Hello nem tervezett feladatátvétel befejezése után összes erőforrás elérhető hello elsődleges hely újra.
   4. Hello elsődleges hely nem érhető el ebben az esetben a Hyper-V Manager konzol hello hello másodlagos hely, engedélyezze a hello VMM virtuális gép fordított replikációját. A másodlagos tooprimary replikálása hello virtuális gép elindul.
   5. A tervezett feladatátvétel végrehajtása hello Hyper-V Manager konzolon hello másodlagos helyen, toofail hello VMM virtuális gépek toohello elsődleges hely alatt. Hello feladatátvételi véglegesítése. A fordított replikáció, majd engedélyezze, hogy hello VMM virtuális gép újra replikálása működik az elsődleges toosecondary.
   6. A Recovery Services-tároló hello engedélyezze a fordított replikáció indítása virtuális gépeket, replikálásához őket másodlagos tooprimary toostart hello munkaterhelés.
   7. A hello Recovery Services-tároló, futtassa a tervezett feladatátvétel toofail hátsó hello munkaterhelési virtuális gépek toohello az elsődleges hely. Hello feladatátvételi toocomplete véglegesítése azt. Engedélyezze a visszirányú replikálás toostart replikációs hello munkaterhelési virtuális gépekhez az elsődleges toosecondary.

### <a name="stretched-vmm-cluster"></a>Felhőbe archivált VMM-fürthöz

Helyett egy önálló VMM-kiszolgáló telepítésének tooa másodlagos helyre replikált virtuális, akkor is használhatja a VMM magas rendelkezésre állású Windows feladatátvevő fürtben virtuális gépként telepíti azokat. Így lehetővé teszi a rugalmas munkaterhelést és a hardver meghibásodása elleni védelem. a Site Recovery hello VMM virtuális gépek toodeploy földrajzilag különálló helyen stretch fürtben kell telepíteni. toodo ezt:

1. Telepítse a VMM egy virtuális gépen egy Windows feladatátvevő fürtben, és válassza a hello beállítás toorun hello kiszolgáló magas rendelkezésre állásúként a telepítés során.
2. a VMM által használt hello SQL Server-példány replikálni kell az SQL Server AlwaysOn rendelkezésre állási csoportok, úgy, hogy hello adatbázis hello másodlagos helyen.
3. Kövesse a cikk toocreate egy tárolót hello utasításait, hello kiszolgáló regisztrálásához és védelmének beállítása. Recovery Services-tároló hello a fürt minden VMM-kiszolgálót a hello tooregister van szüksége. toodo, telepítse a szolgáltató hello aktív csomópontra, és regisztrálja hello VMM-kiszolgálót. Majd telepítse a szolgáltató hello többi csomóponton.
4. Nem tervezett kimaradás esetén hello VMM-kiszolgáló a megfelelő SQL Server-adatbázist a feladatátvételt és hello másodlagos helyről elérhető.



## <a name="prepare-for-storage-mapping"></a>Tárolásleképezés előkészítése


Tárolás beállítása leképezés által leképezési tárhelybesorolások a forrás és cél VMM kiszolgálók toodo hello következő:

  * **Azonosítsa a célként megadott replika virtuális gépek**– A forrás virtuális gép merevlemez replikálja a megadott tárolási toohello (SMB-megosztás vagy a fürt megosztott kötetei (CSV)) hello célként megadott helyen.
  * **A replika virtuális gépek elhelyezése**– tárolási használt toooptimally hely replika virtuális gépek a Hyper-V gazdakiszolgálókra lesz. Replika virtuális gépeket helyez el a gazdagépekhez, amelyek hozzáférhetnek a leképezett hello tárhelybesorolás.
  * **Nincs tárhely leképezés**– Ha nem adja meg tároló leképezési, a virtuális gépek lesznek hello replika virtuális géphez társított hello Hyper-V gazdagép-kiszolgálón megadott replikált toohello alapértelmezett tárolási helyét.

Vegye figyelembe:
- Beállíthat egy kiszolgálón két VMM-felhők közötti leképezést.
- Tárhelybesorolások forrása és célja egy felhőben található rendelkezésre álló toohello gazdagépcsoportok kell lennie.
- Nincs szükség a besorolások toohave hello azonos típusú tárolón. Például SMB megosztások tooa Cél besorolás, amely tartalmazza a megosztott fürtkötetek tartalmazó forrás besorolást is leképezheti.

### <a name="example"></a>Példa
Ha besorolások megfelelően vannak konfigurálva a VMM-ben tárolási leképezés során hello forrás és cél VMM-kiszolgáló kiválasztásakor, hello forrása és célja besorolások jelenik meg. Íme egy példa, tárolási fájlmegosztásokat és besorolása két helyen található Győr és Chicagói rendelkező szervezeteknél.

| **Hely** | **VMM-kiszolgáló** | **A fájlmegosztás (forrás)** | **Besorolási (forrás)** | **Leképezve** | **A fájlmegosztás (cél)** |
| --- | --- | --- | --- | --- | --- |
| New York |VMM_Source |SourceShare1 |ARANY |GOLD_TARGET |TargetShare1 |
| SourceShare2 |EZÜST |SILVER_TARGET |TargetShare2 | | |
| SourceShare3 |BRONZ |BRONZE_TARGET |TargetShare3 | | |
| Chicago |VMM_Target | |GOLD_TARGET |Nincsenek leképezve: | |
|  |SILVER_TARGET |Nincsenek leképezve: | | | |
|  |BRONZE_TARGET |Nincsenek leképezve: | | | |

Az ebben a példában:

* A replika virtuális gép létrehozásakor bármely virtuális gép, GOLD tárhelyen (SourceShare1) lesz replikált tooa GOLD_TARGET storage (TargetShare1).
* EZÜST tárhelyen (SourceShare2) a virtuális gépek a replika virtuális gép létrehozásakor azt replikált tooa SILVER_TARGET (TargetShare2) tároló, és így tovább.

### <a name="multiple-storage-locations"></a>Több tárolóhelyek
Ha hello Cél besorolás toomultiple SMB-megosztások vagy CSV-kben van hozzárendelve, hello optimális tárolási helye automatikusan ki lesz választva védett hello virtuális gép esetében. Ha nincs megfelelő célként megadott esetén érhető el a hello besorolást, hello alapértelmezett megadott tárolási helyére hello Hyper-V gazdagépen használt tooplace hello replika virtuális merevlemezeket.

a következő táblázat hello bemutatják, hogyan tárhelybesorolás és megosztott fürtkötetek beállítása a fenti példában.

| **Hely** | **Besorolás** | **A társított tárolási** |
| --- | --- | --- |
| New York |ARANY |<p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> |
| EZÜST |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p> | |
| Chicago |GOLD_TARGET |<p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p> |
| SILVER_TARGET |<p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p> | |

A táblázat összefoglalja a hello viselkedést, ha engedélyezi a védelmet a virtuális gépek (VM1 - VM5) ebben a példában környezetben.

| **Virtuális gép** | **Forrás tároló** | **Forrás besorolás** | **Hozzárendelt céltárolót** |
| --- | --- | --- | --- |
| VM1 |C:\ClusterStorage\SourceVolume1 |ARANY |<p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Mindkét GOLD_TARGET</p> |
| VM2 VIRTUÁLIS GÉPNEK |\\FileServer\SourceShare1 |ARANY |<p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Mindkét GOLD_TARGET</p> |
| VM3 |C:\ClusterStorage\SourceVolume2 |EZÜST |<p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p> |
| VM4 |\FileServer\SourceShare2 |EZÜST |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Mindkét SILVER_TARGET</p> |
| VM5 |C:\ClusterStorage\SourceVolume3 |N/A |Nincs leképezés, így hello hello Hyper-V gazdagép alapértelmezett tárolási helyét szolgál |



### <a name="data-privacy-overview"></a>Az adatvédelmi áttekintése

A táblázat összefoglalja, ebben a forgatókönyvben az adatok tárolási módját:

- - -
| Műveletek | **Részletek** | **Összegyűjtött adatok** | **Használat** | **Szükséges** |
| --- | --- | --- | --- | --- |
| **Regisztráció** | Recovery Services-tároló rögzítheti egy VMM-kiszolgáló. Ha később szeretné toounregister egy kiszolgálót, akkor ehhez hello kiszolgálóadatok hello Azure-portálon való törlésekor. | A VMM-kiszolgáló regisztrálása után a Site Recovery gyűjt, folyamatok, és hello VMM-kiszolgáló és a Site Recovery által észlelt hello VMM-felhőkben hello nevei metaadatainak továbbítja. | hello adatokat használt tooidentify és hello megfelelő a VMM-kiszolgálóval való kommunikációhoz, és megfelelő VMM-felhő beállításainak konfigurálása. | Ez a szolgáltatás szükség. Ha nem szeretné, hogy toosend ezen információk tooSite helyreállítási azt ne használja a hello Site Recovery szolgáltatásban. |
| **A replikáció engedélyezése** | hello Azure Site Recovery Provider hello VMM-kiszolgálón van telepítve, és hello átjáró való kommunikációhoz hello Site Recovery szolgáltatásban működik. hello szolgáltató egy dinamikus csatolású függvénytár (DLL) üzemeltetett hello VMM folyamatban. Hello szolgáltató van telepítve, után hello "Adatközpont-helyreállítási" lekérdezi engedélyezve van a hello VMM felügyeleti konzolján. Új és meglévő virtuális gépek engedélyezheti a szolgáltatás tooenable védelmet a virtuális gép. |E tulajdonság készlet a szolgáltató hello hello nevét és Azonosítóját hello VM tooSite helyreállítási küld.  Windows Server 2012 vagy Windows Server 2012 R2 Hyper-V replika engedélyezve van a replikáció. hello virtuális gép adatainak beolvasása replikálódnak az egy Hyper-V gazdagép tooanother (általában a különböző "helyreállítási" adatközpontban található). |A Site Recovery hello toopopulate hello virtuális gép metaadatait hello Azure-portált használja. | Ez a szolgáltatás hello szolgáltatás nagyon fontos részét képezik, és nem kapcsolható ki. Ha nem szeretné, toosend ezt az információt, nem engedélyezi a Site Recovery által védett virtuális gépek. Vegye figyelembe, hogy hello szolgáltató tooSite helyreállítási által küldött minden adathoz HTTPS-KAPCSOLATON keresztül zajlik. |
| **Helyreállítási terv** | A helyreállítási terv segíteni az orchestration tervének hello helyreállítási adatközpont. Megadhatja, hogy hello sorrendben, amelyben virtuális gépek vagy virtuális gépek csoportjának el kell indulniuk hello helyreállítási helyen. Bármely automatizált parancsfájlokat toobe futtatni, vagy a manuális műveletet toobe készít, helyreállításkor hello az egyes virtuális gépek is megadható. Feladatátvételi hello helyreállítási terv szinten koordinált helyreállítási általában elindul. | A Site Recovery gyűjti, folyamatokat, és továbbítja a metaadatok hello helyreállítási terv, beleértve a virtuális gép metaadatait, és bármilyen automatizálási parancsfájlokat és a manuális műveletet megjegyzések metaadatait. |hello metaadatai használt toobuild hello helyreállítási tervben szereplő hello Azure-portálon. |Ez a szolgáltatás hello szolgáltatás nagyon fontos részét képezik, és nem kapcsolható ki. Ha az információ tooSite helyreállítási toosend nem szeretné, ne hozzon létre helyreállítási tervek. |
| **Hálózatleképezés** | A Maps hálózati hello elsődleges data center toohello helyreállítási adatközpont adatait. Amikor virtuális gépeket a helyreállítási helyen hello helyreállítás, hálózatra való leképezést a hálózati kapcsolatot lehessen segítségével. |A Site Recovery gyűjti, folyamatokat, és továbbítja a hello metaadatok hello logikai hálózatok (elsődleges és datacenter) minden egyes hely esetében. |hello metaadatai használt toopopulate hálózati beállításokat, hogy is leképezheti hello hálózati. | Ez a szolgáltatás hello szolgáltatás nagyon fontos részét képezik, és nem kapcsolható ki. Ha az információ tooSite helyreállítási toosend nem szeretné, ne használjon hálózatra való leképezés. |
| **A feladatátvétel (tervezett/nem tervezett/vizsgálat)** | Virtuális gépek a VMM által felügyelt adatok egy center tooanother feladatátvevő feladatátadás. hello feladatátvételi művelet hello Azure-portálon manuálisan váltott ki. |hello szolgáltató hello VMM-kiszolgálón a Site Recovery által hello feladatátvételi esemény értesítést kap, egy feladatátvételi művelet futó hello Hyper-V gazdagép VMM felületeken keresztül. A virtuális gépek tényleges feladatátvételt egy Hyper-V gazdagép tooanother származik, és kezeli a Windows Server 2012 vagy Windows Server 2012 R2 Hyper-V replika. Lehetne a Site Recovery toopopulate hello állapotának hello feladatátvételi művelet információkat küldi hello Azure-portálon hello információkat használja. | Ez a szolgáltatás hello szolgáltatás nagyon fontos részét képezik, és nem kapcsolható ki. Ha az információ tooSite helyreállítási toosend nem szeretné, ne használjon feladatátvételi. |

## <a name="next-steps"></a>Következő lépések

Hello telepítés tesztelését, követően további információ más típusú [feladatátvételi](site-recovery-failover.md)
