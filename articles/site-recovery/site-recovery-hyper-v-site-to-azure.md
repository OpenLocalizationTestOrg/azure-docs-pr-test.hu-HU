---
title: "Hyper-V virtuális gépek tooAzure aaaReplicate |} Microsoft Docs"
description: "Ismerteti, hogyan tooorchestrate replikációjának, feladatátvételének és helyreállításának helyszíni Hyper-V virtuális gépek tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 1777e0eb-accb-42b5-a747-11272e131a52
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 04/05/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: hyper-v-site-walkthrough-overview
ms.openlocfilehash: 6fba41e43823fc57511d51ea2e09691159693982
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-tooazure-using-azure-site-recovery-with-hello-azure-portal"></a>Hyper-V virtuális gépek (VMM nélkül) tooAzure hello Azure-portálon az Azure Site Recovery segítségével replikálni.

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [Klasszikus Azure portál](site-recovery-hyper-v-site-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

Ez a cikk ismerteti, hogyan tooreplicate helyszíni Hyper-V virtuális gépek tooAzure, használatával [Azure Site Recovery](site-recovery-overview.md) a hello Azure-portálon. A központi telepítés a Hyper-V virtuális gépek felügyelete nem a System Center Virtual Machine Manager (VMM).

A cikk elolvasása után bármely fűzhetnek hello lap alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Ha azt szeretné, hogy toomigrate gépek tooAzure (nélkül a feladat-visszavétel), további információk: [Ez a cikk](site-recovery-migrate-to-azure.md).



## <a name="deployment-steps"></a>A központi telepítés lépései

Kövesse az alábbi telepítési lépéseket hello cikk toocomplete:

1. [További](site-recovery-components.md#hyper-v-to-azure) kapcsolatos hello architektúra ehhez a központi telepítéshez. Emellett [többet megtudhat](site-recovery-hyper-v-azure-architecture.md) arról, hogyan működik a Hyper-V-replikáció a Site Recoveryben.
2. Előfeltételek és korlátozások ellenőrzése.
3. Azure-hálózat és tárfiókok beállítása.
4. Felkészülés a Hyper-V-gazdagépek.
5. Recovery Services-tároló létrehozása. hello tároló konfigurációs beállításait tartalmazza, és koordinálja a replikációt.
6. Forrásbeállítások megadása. Hyper-V-gazdagépek hello tartalmazó Hyper-V-hely létrehozása, és regisztrálja hello hely hello tárolóban. Telepítse az Azure Site Recovery Provider hello és hello Microsoft Recovery Services Agent ügynököt, hello Hyper-V gazdagépeken.
7. Adja meg a cél- és replikációs beállításokat.
8. Engedélyezze a hello virtuális gépek replikációját.
9. Futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik.



## <a name="prerequisites"></a>Előfeltételek


**Követelmény** | **Részletek** |
--- | --- |
**Azure** | Az [Azure-követelmények](site-recovery-prereq.md#azure-requirements) ismertetése.
**Helyszíni kiszolgálók** | [További](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager) hello helyszíni Hyper-V-gazdagépek követelményeivel kapcsolatos.
**Helyszíni Hyper-V rendszerű virtuális gépek** | Virtuális gépek kívánt tooreplicate rendszerűnek kell lennie egy [támogatott operációs rendszer](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), és adott a specifikációknak való [Azure-Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Azure-beli URL-címek** | Hyper-V-gazdagépek toothese URL-címeket kell elérni:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Ha IP-címeken alapuló tűzfalszabályok szabály, győződjön meg arról, kommunikációs tooAzure lehetővé teszik.<br/></br> Hello engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443) port hello.<br/></br> Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).



## <a name="prepare-for-deployment"></a>Felkészülés az üzembe helyezésre

központi telepítés kell tooprepare:

1. [Állítsa be az Azure-hálózatot](#set-up-an-azure-network) , amely Azure virtuális gépek megjelenik a található ha feladatátvételt követően jönnek létre.
2. [Állítsa be az Azure-tárfiókot](#set-up-an-azure-storage-account) a replikált adatok tárolásához.
3. [Készítse elő a hello Hyper-V gazdagépek](#prepare-the-hyper-v-hosts) tooensure hello eléréséhez szükséges URL-címeket.

### <a name="set-up-an-azure-network"></a>Azure-hálózat beállítása

Állítsa be az Azure-hálózatot. Ez szüksége, hogy a létrehozott követően feladatátvételi csatlakoztatott tooa hálózati hello Azure virtuális gépek.

* hello hello hálózat legyen ugyanabban a régióban, hello Recovery Services-tároló.
* Attól függően, hogy az erőforrás-modellje hello meg toouse az Azure virtuális gépek a feladatátvételt, beállított hello Azure hálózati [Resource Manager módra](../virtual-network/virtual-networks-create-vnet-arm-pportal.md), vagy [klasszikus módban](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
* Javasoljuk, hogy a kezdés előtt hozza létre a hálózatot. Ha ezt elmulasztja, toodo kell azt a Site Recovery üzembe helyezése során.
- Nem lehet a Site Recovery által használható Storage-fiókok [áthelyezése](../azure-resource-manager/resource-group-move-resources.md) belül hello azonos, vagy másik, előfizetések között.


### <a name="set-up-an-azure-storage-account"></a>Azure-tárfiók beállítása

- A standard vagy prémium replikálja tooAzure toohold adatok Azure storage-fiók szükséges. [Prémium szintű storage](../storage/storage-premium-storage.md) jellemzően egy folyamatosan magas I/O teljesítménye és kis késleltetésű toohost IO-igényes munkaterhelések igénylő virtuális gépektől.
- Ha azt szeretné, hogy toouse egy prémium szintű fiók toostore replikált adatokból, is szükség van egy standard tárolási fiók toostore replikálási naplókhoz, hogy a folyamatban lévő rögzítési tooon helyszíni adatokhoz módosítja.
- Attól függően, hogy az erőforrás-modellje hello meg toouse az Azure virtuális gépek a feladatátvételt, a fiók beállítása [Resource Manager módra](../storage/storage-create-storage-account.md), vagy [klasszikus módban](../storage/storage-create-storage-account-classic-portal.md).
- Azt javasoljuk, hogy állítsa be a tárfiók megkezdése előtt. Ha most kihagyja ezt toodo kell azt a Site Recovery üzembe helyezése során. hello fiókoknak kell lenniük a hello hello és ugyanabban a régióban Recovery Services-tároló.
- Nem lehet áthelyezni a storage-fiókok keresztül által használt Site Recovery erőforráscsoportok hello belül azonos előfizetés, vagy különböző előfizetésekhez.


### <a name="prepare-hello-hyper-v-hosts"></a>Hello Hyper-V-gazdagépek előkészítése

* Győződjön meg arról, hogy hello Hyper-V gazdagépek megfelelnek-e a hello [Előfeltételek](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager).
- Győződjön meg arról, hogy a hello állomások hozzáférhet-e a szükséges hello URL-címeket.


## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson az **Új** > **Felügyelet és kezelés** > **Backup és Site Recovery (OMS)** lehetőségre.  

    ![Új tároló](./media/site-recovery-hyper-v-site-to-azure/new-vault.png)

3. A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban. Ha egynél több előfizetéssel rendelkezik, válasszon egyet ezek közül.

4. [Hozzon létre egy új erőforráscsoportot](../azure-resource-manager/resource-group-template-deploy-portal.md) , vagy válasszon egy meglévőt, és adja meg a kívánt Azure-régiót. Gépek replikált toothis terület lesz. támogatott toocheck régiókban, tekintse meg a cikknek a földrajzi elérhetőséggel [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/).

5. Tooquickly hozzáférés hello tároló a hello irányítópult, kattintson a **PIN-kód toodashboard**, és kattintson a **létrehozása**.

    ![Új tároló](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

hello új tároló megjelenik hello **irányítópult** > **összes erőforrás** listában, és a hello fő **Recovery Services-tárolók** panelen.



## <a name="select-hello-protection-goal"></a>Hello védelmi cél kiválasztása

Jelölje ki a tooreplicate, és a tooreplicate, ahová.

1. A hello **Recovery Services-tárolók**, jelölje be hello tárolóban.
2. A **Bevezetés** ablakban kattintson a **Site Recovery** > **Az infrastruktúra előkészítése** > **Védelmi cél** lehetőségre.

    ![Célok megválasztása](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)
3. A **védelmi cél**, jelölje be **tooAzure**, és válassza ki **Igen, a Hyper-V-vel**. Válassza ki **nem** tooconfirm a VMM nem használja. Ezután kattintson az **OK** gombra.

    ![Célok megválasztása](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)

## <a name="set-up-hello-source-environment"></a>Hello forráskörnyezet beállítása

Hello Hyper-V hely beállítása hello Azure Site Recovery Provider és hello Azure Recovery Services Agent ügynök telepítése a Hyper-V gazdagépek és hello hely regisztrálja hello tárolóban.

1. A **infrastruktúra előkészítése**, kattintson a **forrás**. a Hyper-V gazdagépekhez vagy fürtökhöz, tárolója új Hyper-V hely tooadd kattintson **+ Hyper-V hely**.

    ![A forrás beállítása](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)
2. A **hozzon létre Hyper-V hely**, adja meg a hello hely nevét. Ezután kattintson az **OK** gombra. Jelölje ki, hello helyen létrehozott, és kattintson a **+ Hyper-V Server** tooadd egy kiszolgáló toohello helyet.

    ![A forrás beállítása](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. A **kiszolgáló hozzáadása** > **kiszolgálótípus**, ellenőrizze, hogy **Hyper-V server** jelenik meg.

    - Győződjön meg arról, hogy hello Hyper-V-kiszolgálónak tooadd hello megfelel [Előfeltételek](#on-premises-prerequisites), és képes tooaccess hello van megadott URL-címeket.
    - Töltse le a hello Azure Site Recovery Provider telepítőfájlját. A fájl tooinstall hello szolgáltatót futtatja, és minden Hyper-V gazdagépen a Recovery Services agent hello.

    ![A forrás beállítása](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)


## <a name="install-hello-provider-and-agent"></a>Hello szolgáltató és az ügynök telepítése

1. Futtassa a szolgáltató telepítőfájl hello minden állomáson hozzáadott toohello Hyper-V helyet. Hyper-V fürt telepítése, futtassa a telepítőt a fürt minden csomópontján. Telepítése és regisztrálása a Hyper-V fürt minden csomópontján biztosítja a virtuális gépek védelmének biztosításához, akkor is, ha az áttelepítés után csomópontjai között.
2. A **Microsoft Update** lapon kérheti a frissítések beszerzését, így a rendszer a Microsoft Update-szabályzatnak megfelelően telepíteni fogja a Providerhez kiadott frissítéseket.
3. A **telepítési**, fogadja el vagy módosítsa a hello Provider alapértelmezett telepítési helyét, majd kattintson **telepítése**.
4. A **tároló beállításait**, kattintson a **Tallózás** tooselect hello tároló kulcsfájlját letöltött. Adja meg a hello Azure Site Recovery-előfizetést, hello tároló nevére, és hello Hyper-V hely toowhich hello Hyper-V kiszolgáló tartozik.

    ![Kiszolgáló regisztrációja](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5. A **proxybeállítások**, adja meg, hogyan Hyper-V gazdagépeken futó szolgáltató az internetre csatlakozik a Site Recovery tooAzure hello hello internet.

    * Ha azt szeretné, hello szolgáltató tooconnect közvetlenül válassza **csatlakozzon közvetlenül a Site Recovery tooAzure proxy nélkül**.
    * Ha az aktuálisan használt proxy hitelesítést igényel, vagy hello szolgáltatói kapcsolat toouse egyéni proxyt használni szeretne, válassza ki a **csatlakozzon a Site Recovery proxykiszolgálóval tooAzure**.
    * Ha proxyt használ:
        - Hello cím, port és a hitelesítő adatok megadása
        - Győződjön meg arról, hogy hello URL-címeket a hello [Előfeltételek](#prerequisites) hello proxyn keresztül történő használata engedélyezett.

    ![internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6. Kattintson a telepítés befejezése után **regisztrálása** tooregister hello server hello tárolóban lévő állapottal.

    ![Telepítés helye](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7. Regisztráció befejezése után a metaadatok hello Hyper-V kiszolgálóról Azure Site Recovery lekéri, és hello kiszolgálóhoz **Site Recovery-infrastruktúra** > **Hyper-V-gazdagépek**.


## <a name="set-up-hello-target-environment"></a>Hello célkörnyezet beállítása

Adja meg a hello Azure storage-fiók replikációhoz, és hello Azure hálózati toowhich Azure virtuális gépek a feladatátvételt követően csatlakozhatnak.

1. Kattintson a **infrastruktúra előkészítése** > **cél**.
2. Válassza ki a hello előfizetés és hello erőforráscsoportot használni kívánt toocreate hello Azure virtuális gépek a feladatátvételt követően. Válassza ki a hello telepítési modell, amelyet az Azure (klasszikus vagy erőforrás-kezelés) toouse hello virtuális gépek számára.

3. A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.

    - Ha a tárfiók nincs, kattintson a **+ tárolás** toocreate egy erőforrás-kezelő fiók beágyazott. További információ a [tárhellyel kapcsolatos követelmények](site-recovery-prereq.md#azure-requirements).
    - Ha nem rendelkezik Azure-hálózatot, kattintson a **+ hálózat** toocreate a Resource Manager-alapú hálózati beágyazott.

    ![Storage](./media/site-recovery-vmware-to-azure/enable-rep3.png)




## <a name="configure-replication-settings"></a>Replikációs beállítások konfigurálása

1. Kattintson egy új replikációs házirendet toocreate **infrastruktúra előkészítése** > **replikációs beállítások** > **+ létrehozás és társítás**.

    ![Network (Hálózat)](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)
2. A **Házirend létrehozása és társítása** beállításnál adja meg a szabályzat nevét.
3. A **másolás gyakorisága**, adja meg, milyen gyakran tooreplicate különbözeti adatokat hello kezdeti replikálás (30 másodperces, 5 vagy 15 perc) után.

    > [!NOTE]
    > Az 30 második gyakoriságát toopremium tárolási replikálása esetén nem támogatott. hello korlátozás hello pillanatképek számát / (100) blob támogatja prémium szintű storage határozza meg. [További információk](../storage/storage-premium-storage.md#snapshots-and-copy-blob).

4. A **helyreállításipont-megőrzést**, mennyi ideig óra az egyes helyreállítási pontok áll hello adatmegőrzési időtartam. Virtuális gépek lehet helyreállítani egy időszakban tooany pont.
5. A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg, hogy milyen gyakran (1 – 12 órába) helyreállítási pontokat tartalmazó alkalmazáskonzisztens pillanatképeket jönnek létre.

    - Hyper-V két különböző használ – standard pillanatkép, amely lefedő növekményes pillanatképet hello teljes virtuális gép, és egy alkalmazáskonzisztens pillanatkép, amely időpontban pillanatképet készít a hello alkalmazásadatok hello virtuális gépen belül.
    - Alkalmazáskonzisztens pillanatképek a kötet árnyékmásolata szolgáltatás (VSS) tooensure, amely az alkalmazások konzisztens állapotban legyenek, hello pillanatkép készítésének időpontjában.
    - Ha engedélyezi az alkalmazáskonzisztens pillanatképeket, az hatással hello forrás virtuális gépeken futó alkalmazások teljesítményére. Győződjön meg arról, hogy hello beállított érték kisebb, mint hello további helyreállítási pontok száma.

6. A **kezdeti replikáció kezdési ideje**, adja meg, amikor toostart hello kezdeti replikálása. hello replikálást, az internetes sávszélességet, ezért érdemes tooschedule azt a foglalt munkaidőn kívül. Ezután kattintson az **OK** gombra.

    ![Replikációs szabályzat](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Ha egy új házirendet hoz létre, azt automatikusan hello Hyper-V hely társítja. A Hyper-V hely (és a benne lévő virtuális gépek hello) társíthatja a több replikációs házirend **replikációs** > házirend neve > **társítása Hyper-V hely**.

## <a name="capacity-planning"></a>Kapacitástervezés

Most, hogy az alapvető infrastruktúra beállítása, gondolja át a kapacitástervezés és mérje fel, hogy mikor szükséges további erőforrások.

A Site Recovery biztosít a kapacitás planner toohelp hello erőforrások a számítási, hálózati és tárolási osszon ki. Futtathatja hello planner a virtuális gépek, a lemezek és a tárolás átlagos számán alapuló becsléseket gyors módját, vagy a részletes módot, az egyéni számmal rendelkező hello munkaterhelés szinten. Megkezdése előtt kell:

* Gyűjtse össze a replikálási környezetre vonatkozó adatokat, többek között a virtuális gépeket, a virtuális gépekhez tartozó lemezeket, valamint a lemezenkénti tárterületeket.
* A replikált adatok hello napi adatváltozás (forgalom) arány becslése. Használhatja a hello [Capacity Planner for Hyper-V replika](https://www.microsoft.com/download/details.aspx?id=39057) toohelp ehhez.

1. Kattintson a **letöltése** toodownload hello eszközt, és futtassa azt. [A cikk elolvasása hello](site-recovery-capacity-planner.md) , amely kísérik hello eszköz.
2. Amikor elkészült, válassza ki a **Igen** a **futtatta hello Capacity Planner**?

   ![Kapacitástervezés](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

További információk [a hálózati sávszélesség szabályozásáról](#network-bandwidth-considerations)



## <a name="enable-replication"></a>A replikáció engedélyezése

Mielőtt elkezdené, győződjön meg arról, hogy az Azure felhasználói fiók rendelkezik-e a szükséges hello [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) egy új virtuális gép tooAzure tooenable replikációját.

Engedélyezze a virtuális gépek replikálása az alábbiak szerint:          

1. Kattintson a **alkalmazás replikálása** > **forrás**. Először hello replikációjának beállítása után kattintson **+ replikálás** a további gépek replikációjának tooenable.

    ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)
2. A **forrás**, jelölje be hello Hyper-V helyet. Ezután kattintson az **OK** gombra.
3. A **cél**, válasszon hello tároló előfizetést és hello feladatátvételi típusú a toouse (klasszikus vagy erőforrás-kezelés) Azure-ban a feladatátvételt követően.
4. Válassza ki a kívánt toouse hello tárfiókot. Ha azt szeretné, hogy toouse fiók nem rendelkezik, akkor [hozzon létre egyet](#set-up-an-azure-storage-account). Ezután kattintson az **OK** gombra.
5. SELECT hello Azure hálózati és alhálózati toowhich Azure virtuális gépeken fog csatlakozni, ha feladatátvételi jönnek létre.

    - Válassza ki a tooapply hello hálózati beállítások tooall gépek engedélyezi a replikáció, **beállítás most a kijelölt gépekhez**.
    - Válassza ki **konfigurálását később** tooselect hello gépenként Azure-hálózatot.
    - Ha azt szeretné, hogy toouse hálózati nincs, akkor [hozzon létre egyet](#set-up-an-azure-network). Ha szükséges, válassza ki az alhálózatot. Ezután kattintson az **OK** gombra.

   ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. A **virtuális gépek** > **válassza ki a virtuális gépek**kattintson, és válassza az egyes gépek tooreplicate szeretné. Csak olyan gépeket választhat, amelyeken használható a replikáció funkció. Végül kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/enable-replication5-for-exclude-disk.png)

7. A **tulajdonságok** > **tulajdonságainak konfigurálása**hello operációs rendszert válasszon ki a kijelölt hello virtuális gépek és operációsrendszer-lemez hello.
8. Győződjön meg arról, hogy hello Azure virtuális gép nevét (cél neve) megfelel [Azure virtuális gép követelményeinek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
9. Alapértelmezés szerint az összes hello lemez a virtuális gép hello replikációhoz vannak kijelölve, de a lemezek tooexclude törölheti őket.
    - Érdemes lehet tooexclude lemezek tooreduce replikáció sávszélesség. Például előfordulhat, hogy nem kívánja tooreplicate lemezek ideiglenes adatokkal, vagy adatokat, minden alkalommal, amikor frissítette a gépek vagy alkalmazások (például pagefile.sys vagy Microsoft SQL Server tempdb) újraindul. Unselecting hello lemez hello lemez lehet kizárni a replikálásból.
    - Csak az alaplemezek zárhatók ki. Operációsrendszer-lemezek nem zárhatók ki.
    - Azt javasoljuk, hogy ne zárja ki a dinamikus lemezeket. A Site Recovery nem tudja azonosítani, hogy egy virtuális merevlemez a vendég virtuális gépben alap- vagy dinamikus lemez-e. Ha az összes függő dinamikus kötet lemez nem zárható ki, hello védett dinamikus lemez állapotúként lesz megjelölve, a hibás lemez hello VM átadja a feladatokat, és a lemezen levő hello adatok nem lesz elérhető.
        - A replikáció engedélyezése után már nem lehet ahhoz lemezeket hozzáadni, vagy lemezeket eltávolítani belőle. Ha szeretné, hogy tooadd, vagy zárja ki egy lemezt, toodisable védelmi hello VM van szükség, és majd engedélyezze újra.
        - Az Azure-ban manuálisan létrehozott lemezek nem vesznek részt a feladatátvételben. Például ha több mint három lemezt sikertelen, és hozzon létre két közvetlenül az Azure virtuális Gépen, csak hello három lemezeket, amelyeket a feladatátvételt volt sikertelen lesz újból az Azure tooHyper-V. Feladat-visszavétel, vagy a fordított replikálást a Hyper-V tooAzure manuálisan létrehozott lemezek nem adhat meg.
        - Ha kizárja a olyan lemezzel, amelyet egy alkalmazás toooperate van szükség, miután feladatátvételi tooAzure kell toocreate manuálisan az Azure, így az adott hello replikálva alkalmazás futtatható. Azt is megteheti az Azure automation sikerült integrálja a helyreállítási terv toocreate hello lemez hello gép feladatátvétele során.

10. Kattintson a **OK** toosave módosításokat. A további tulajdonságokat később is beállíthatja.

    ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/enable-replication6-with-exclude-disk.png)

11. A **replikációs beállítások** > **replikáció beállításainak konfigurálása**, válassza ki a kívánt tooapply hello védett virtuális gépek hello replikációs házirend. Ezután kattintson az **OK** gombra. Módosíthatja a hello replikációs házirendet a **replikációs házirendek** > házirend neve > **beállításainak szerkesztése**. Az itt megadott módosítások a már replikálás alatt álló, illetve újonnan hozzáadott gépekre egyaránt érvényesek lesznek.


   ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

Hello állapotának nyomon követheti **Védelemengedélyezési** feladat **feladatok** > **Site Recovery-feladatok**. Hello után **Védelemvéglegesítési** feladat futtatása hello gép készen áll a feladatátvételre.

### <a name="view-and-manage-vm-properties"></a>A virtuális gépek tulajdonságainak megtekintése és kezelése

Azt javasoljuk, hogy ellenőrizze a hello forrásgép hello tulajdonságait.

1. A **védett elemek**, kattintson a **replikált elemek**, és jelölje be hello gép.

    ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)
2. A **tulajdonságok**, megtekintheti a replikációs és feladatátvételi adatait hello virtuális gép.

    ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)
3. A **számítás és hálózat** > **számítási tulajdonságok**, megadhatja a hello Azure virtuális gép nevét és a cél méretét. Ha szeretné, módosítsa a hello neve toocomply az Azure-követelményeknek. Is megtekintheti, és információt hello célhálózat, alhálózati és toohello Azure virtuális Géphez rendelendő IP-cím módosításához. Vegye figyelembe a következőket hello:

   * Beállíthat hello cél IP-címet. Ha nem ad meg egy címet, a gép a feladatátvételt hello DHCP fogja használni. Ha egy címet, amely nem használható feladatátadásra, hello feladatátvétel sikertelen lesz. cél IP-cím a feladatátvételi teszthez használható, ha hello címkészletében nem áll rendelkezésre a feladatátvételi teszt hálózatában hello hello.
   * hálózati adapterek száma hello hello mérete határozza meg hello cél virtuális gépre, az alábbiak szerint határozza meg:

     * Ha hálózati adapterek hello forrásgépen hello száma kisebb vagy egyenlő, mint hello cél gép méretéhez engedélyezett adapterek toohello számát, majd hello cél lesz hello ugyanannyi adapter hello forrásaként.
     * Ha hello hello forrás virtuális gép adaptereinek száma meghaladja hello engedélyezett hello célméretet majd hello cél maximális fogja használni.
     * Ha például a forrásgépen két hálózati adapterrel és hello célgép mérete négy támogatja, hello célgépen két adapter fog működni. Ha hello forrásgépen két adapter, azonban a hello támogatott célméretet csak egy majd hello célgépen csak egy adapter fog működni.     
     * Ha hello virtuális gép több hálózati adapter lesz összes csatlakozáshoz toohello ugyanazon a hálózaton.
     * Ha hello a virtuális gépnek a több hálózati adapter, majd hello először egy hello listán látható lesz hello *alapértelmezett* hello Azure virtuális gép hálózati adapteréhez.

     ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

4. A **lemezek**, megtekintheti a hello operációs rendszer és az adatlemezek hello virtuális Gépet, amely a rendszer replikálja.

#### <a name="managed-disks"></a>Felügyelt lemezek

A **számítás és hálózat** > **számítási tulajdonságok**, ha azt szeretné, hogy tooattach felügyelt lemezek tooyour gép áttelepítési tooAzure "Az kezelt lemezek" beállítás túl "Yes" a virtuális gép hello állíthatja be. Kezelt lemezeken Azure IaaS virtuális gépek Lemezkezelés leegyszerűsíti hello virtuális gépek lemezei társított hello storage-fiókok kezelése. [További információk a felügyelt lemezekről](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview).

   - Felügyelt lemezek létrehozott és csatolt toohello virtuális gép csak egy feladatátvételi tooAzure. A védelem engedélyezése után a helyszíni gépeket adatokat továbbra is tooreplicate toostorage fiókok.
   Felügyelt lemezek csak hello Resource manager üzembe helyezési modellben használatával telepített virtuális gépeket hozható létre.

  > [!NOTE]
  > Feladat-visszavétel Azure tooon helyszíni Hyper-V környezetben jelenleg nem támogatott a felügyelt lemezek gépek. Állítsa be "Az kezelt lemezek" túl "Yes" csak akkor, ha azt tervezi, toomigrate ez gépen tooAzure.

   - Ha úgy állítja be "Az kezelt lemezek" túl "Yes", csak rendelkezésre állási készletek hello erőforráscsoportban "Az kezelt lemezek" készlettel túl "Yes" lenne kijelölésnél érhetők el. Ennek az az oka felügyelt lemezzel rendelkező virtuális gépek csak része lehet "Kezelt lemez" tulajdonságkészlet rendelkezésre állási készletek túl "Yes". Győződjön meg arról, hogy hozzon létre rendelkezésre állási csoportok "Kezelt lemez" tulajdonság beállítva a felügyelt leképezési toouse lemezek a feladatátvételi alapján. Hasonlóképpen ha úgy állítja be "Az kezelt lemezek" túl "nem", csak rendelkezésre állási készletek hello erőforráscsoportban, "Az kezelt lemezek" tulajdonsága túl "nem" lenne kijelölésnél érhetők el. [További információk a felügyelt lemezekről és a rendelkezésre állási csoportokról](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Ha replikációhoz használt hello tárfiók volt titkosítva Storage szolgáltatás titkosítási bármikor időben, a felügyelt lemezek feladatátvétel során sikertelen lesz. Akkor is, vagy állítsa be "Az kezelt lemezek" túl "nem", és ismételje meg a feladatátvételi vagy tiltsa le a védelmet a virtuális gép hello és a védelmét, tooa storage-fiók, amelynek nincs Storage szolgáltatás titkosítási bármikor időben engedélyezve van.
  > [További információk a Storage Service Encryptionről és a felügyelt lemezekről](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="test-hello-deployment"></a>Hello központi telepítés tesztelése

tootest hello telepítési futtathatja egyetlen virtuális gép vagy egy vagy több virtuális gépet tartalmazó helyreállítási terv feladatátvételi tesztjét.

### <a name="before-you-start"></a>Előkészületek

 - Ha azt szeretné, hogy a virtuális gépek tooconnect tooAzure, feladatátvételt követően RDP segítségével ismerje [tooconnect előkészítése](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - az Active Directory és a DNS-toocopy van szüksége a tesztkörnyezetben toofully tesztje. [További információk](site-recovery-active-directory.md#test-failover-considerations).

### <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása

1. toofail keresztül egyetlen számítógépen, a **replikált elemek**, kattintson a virtuális gép hello > **+ feladatátvételi teszt** ikonra.
2. a helyreállítás alatt toofail tervez, a **helyreállítási tervek**, kattintson a jobb gombbal hello terv > **feladatátvételi teszt**. a helyreállítási terv toocreate [kövesse ezeket az utasításokat](site-recovery-create-recovery-plans.md).
3. A **feladatátvételi teszt**, jelölje be hello Azure hálózati toowhich Azure virtuális gépek csatlakoznak feladatátvételt követően.
4. Kattintson a **OK** toobegin hello feladatátvételi. Előrehaladásának parancsával hello VM tooopen tulajdonságát, vagy a hello **feladatátvételi teszt** feladatot a tároló neve > **feladatok** > **Site Recovery-feladatok**.
5. Hello feladatátvétel befejezése után meg kell jelennie toosee hello replika Azure machine hello Azure-portálon jelenik meg > **virtuális gépek**. Győződjön meg arról, hogy a virtuális gép hello hello megfelelő méretét, csatlakoztatva van a megfelelő hálózati toohello, és fut-e.
6. Ha elvégezte a feladatátvételt követően, meg kell tudni tooconnect toohello Azure virtuális Gépen.
7. Ha elkészült, kattintson a **karbantartása a feladatátvételi teszt** a hello helyreállítási terv. A **megjegyzések** és hello feladatátvételi teszthez kapcsolatos megfigyelések feljegyzéséhez mentéséhez. Ezzel a lépéssel törli a teszt feladatátvétele során létrehozott hello virtuális gépek.

További tudnivalókért olvassa el a hello [teszt feladatátvételi tooAzure](site-recovery-test-failover-to-azure.md) cikk.



## <a name="monitor-hello-deployment"></a>A figyelő hello központi telepítés

A figyelő hello konfigurációs beállításokat, és állapotának a Site Recovery üzembe helyezésére:

1. Kattintson a hello tároló neve tooaccess hello **Essentials** irányítópult. Az irányítópulton követheti a Site Recovery feladatok, replikációs állapotát, a helyreállítási terv, kiszolgáló állapotára és események.  

    ![Alapvető erőforrások](./media/site-recovery-hyper-v-site-to-azure/essentials.png)
2. A hello **állapotfigyelő** csempén a helyrendszer-kiszolgálók, amelyek problémát tapasztal, és hello keletkező Site Recovery által hello az elmúlt 24 órában. Testre szabhatja Essentials tooshow hello csempék és elrendezések, amelyek a leghasznosabb tooyou, ideértve más helyreállítás és biztonsági mentési tárolók hello állapotát.
3. Kezelheti és a replikáció a hello **replikált elemek**, **helyreállítási tervek**, és **Site Recovery-feladatok** csempék. További részleteket a feladatok részletezhető **feladatok** > **Site Recovery-feladatok**.

## <a name="command-line-provider-and-agent-installation"></a>Parancssori Provider és agent telepítése

hello Azure Site Recovery Provider és agent is telepíthető a következő parancssori hello segítségével. Ez a módszer egy Server Core a Windows Server 2012 R2 használt tooinstall hello szolgáltató is lehet.

1. Töltse le a hello szolgáltató telepítési és a regisztrációs kulcs tooa mappát. A mappa lehet például a következő: C:\ASR.
2. Egy rendszergazda jogú parancssorból futtassa ezen parancsok tooextract hello Provider telepítőjét:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Ez a parancs tooinstall hello összetevők futtatása:

            C:\ASR> setupdr.exe /i
4. Futtassa ezen parancsok tooregister hello server hello tárolóban:

            CD C:\Program Files\Microsoft Azure Site Recovery Provider\
            C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file>

<br/>
Az elemek magyarázata:

* **/ Credentials**: kötelező paraméter, amely hello helyet, mely hello a regisztrációs kulcs fájlját található  
* **/ FriendlyName**: hello Hyper-V gazdakiszolgáló hello Azure Site Recovery portálon megjelenő nevét hello paramétert kötelező megadni.
* **/ proxyaddress**: nem kötelező paraméter, amely hello hello proxykiszolgáló címét.
* **/ proxyport** : nem kötelező paraméter, amely hello proxykiszolgáló hello port.
* **/ proxyusername**: nem kötelező paraméter, amely hello Proxy felhasználónevét határozza meg, (Ha a proxy hitelesítést igényel).
* **/ proxypassword**: nem kötelező paraméter meghatározza, hogy a jelszó hello hello proxykiszolgáló hitelesítéséhez (Ha a proxy hitelesítést igényel).


## <a name="network-bandwidth-considerations"></a>Hálózati sávszélességgel kapcsolatos szempontok
Használhatja a hello [Hyper-V kapacitás planner eszköz](site-recovery-capacity-planner.md) toocalculate hello sávszélesség (kezdeti replikálása, majd különbözeti) replikációs van szüksége. toocontrol hello összeg replikáció sávszélesség-használati több lehetősége van:

* **A sávszélesség szabályozását**: tooAzure replikáló Hyper-V-forgalom halad át egy adott Hyper-V gazdagépen. Beállíthatja a gazdagép-kiszolgálón hello sávszélesség szabályozását.
* **Sávszélesség finomhangolása**: beállításkulcsok több replikációhoz használt hello sávszélességet is szabályozhatja.

### <a name="throttle-bandwidth"></a>Sávszélesség szabályozása
1. Hello Microsoft Azure Backup szolgáltatás MMC beépülő modul megnyitásához hello Hyper-V gazdagép-kiszolgálón. Alapértelmezés szerint a Microsoft Azure Backup parancsikonja áll rendelkezésre, hello asztalon vagy a C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Kattintson a beépülő modul hello **tulajdonságainak módosítása**.
3. A hello **sávszélesség-szabályozási** lapon válassza **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél**, és állítsa be a munkaórákra hello és a munkaórákon kívüli időre. Érvényes tartomány: az 512 kbit/s too102 MB / s.

    ![Sávszélesség szabályozása](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

Is használhatja a hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) parancsmag tooset sávszélesség-szabályozás. Íme egy példa:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

A **Set-OBMachineSetting -NoThrottle** beállítás azt jelenti, hogy nincs szükség szabályozásra.

### <a name="influence-network-bandwidth"></a>A hálózati sávszélesség szabályozása
1. Hello beállításjegyzék lépjen a túl**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * tooinfluence hello sávszélesség forgalom replikációs lemezen, módosítsa a hello érték hello **UploadThreadsPerVM**, vagy hozzon létre hello kulcsot, ha még nem létezik.
   * az Azure, a feladatátvételi forgalomhoz tooinfluence hello sávszélesség módosítható hello értéke **DownloadThreadsPerVM**.
2. hello alapértelmezett értéke 4. "A szükségesnél több erőforrással" hálózatban ezek a kulcsok hello alapértelmezett értékekhez módosítani kell. hello maximális 32. Forgalom toooptimize hello érték figyelése.

## <a name="next-steps"></a>Következő lépések

Kezdeti replikálás befejeződik, és hello telepítés tesztelését, után hívhatja meg a feladatátvételek hello szükség szerint. [További](site-recovery-failover.md) feladatátvételek különböző típusú, és hogyan toorun őket.
