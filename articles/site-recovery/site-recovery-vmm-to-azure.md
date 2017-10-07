---
title: "a VMM-ben a Hyper-V virtuális gépek aaaReplicate felhők tooAzure |} Microsoft Docs"
description: "Replikációra, a feladatátvétel és a System Center VMM-felhők tooAzure felügyelt Hyper-V virtuális gépek helyreállítása"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: 84182fe4b63862ac7643208a22f236a7515a25a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-site-recovery-in-hello-azure-portal"></a>A VMM-felhők tooAzure hello Azure-portálon a Site Recovery segítségével a Hyper-V virtuális gépek replikálása
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [Klasszikus Azure portál](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Klasszikus PowerShell](site-recovery-deploy-with-powershell.md)


Ez a cikk ismerteti, hogyan tooreplicate helyszíni Hyper-V virtuális gépek kezelése a System Center VMM-felhők tooAzure, hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Ha azt szeretné, hogy toomigrate gépek tooAzure (nélkül a feladat-visszavétel), további információk: [Ez a cikk](site-recovery-migrate-to-azure.md).


## <a name="deployment-steps"></a>A központi telepítés lépései

Kövesse az alábbi telepítési lépéseket hello cikk toocomplete:


1. [További](site-recovery-components.md) kapcsolatos hello architektúra ehhez a központi telepítéshez. Emellett [többet megtudhat](site-recovery-hyper-v-azure-architecture.md) arról, hogyan működik a Hyper-V-replikáció a Site Recoveryben.
2. Előfeltételek és korlátozások ellenőrzése.
3. Azure-hálózat és tárfiókok beállítása.
4. Hello helyszíni VMM-kiszolgáló és a Hyper-V-gazdagépek előkészítése.
5. Recovery Services-tároló létrehozása. hello tároló konfigurációs beállításait tartalmazza, és koordinálja a replikációt.
6. Forrásbeállítások megadása. Hello VMM-kiszolgáló regisztrálása hello tárolóban. Hello Azure Site Recovery Provider telepítését hello VMM kiszolgáló telepítése hello Microsoft Recovery Services Agent ügynököt a Hyper-V gazdagépeken.
7. Adja meg a cél- és replikációs beállításokat.
8. Engedélyezze a hello virtuális gépek replikációját.
9. Futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik.



## <a name="prerequisites"></a>Előfeltételek


**Támogatási követelmények** | **Részletek**
--- | ---
**Azure** | Az [Azure-követelmények](site-recovery-prereq.md#azure-requirements) ismertetése.
**Helyszíni kiszolgálók** | [További](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-azure) hello helyszíni VMM-kiszolgáló és a Hyper-V-gazdagépek követelményeivel kapcsolatos.
**Helyszíni Hyper-V rendszerű virtuális gépek** | Virtuális gépek kívánt tooreplicate rendszerűnek kell lennie egy [támogatott operációs rendszer](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), és adott a specifikációknak való [Azure-Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Azure-beli URL-címek** | hello VMM-kiszolgáló toothese URL-címeket kell elérni:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Ha IP-címeken alapuló tűzfalszabályok szabály, győződjön meg arról, kommunikációs tooAzure lehetővé teszik.<br/></br> Hello engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443) port hello.<br/></br> Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).


## <a name="prepare-for-deployment"></a>Felkészülés az üzembe helyezésre
központi telepítés tooprepare, kell:

1. [Állítson be egy Azure-hálózatot](#set-up-an-azure-network), amely az Azure virtuális gépeket fogja tartalmazni a feladatátvétel után.
2. [Állítsa be az Azure-tárfiókot](#set-up-an-azure-storage-account) a replikált adatok tárolásához.
3. [Készítse elő a VMM-kiszolgáló hello](#prepare-the-vmm-server) a Site Recovery üzembe helyezése.
4. Készüljön fel a hálózatleképezésre. Állítsa be úgy a hálózatokat, hogy a Site Recovery üzembe helyezésekor lehessen konfigurálni a hálózatleképezéseket.

### <a name="set-up-an-azure-network"></a>Azure-hálózat beállítása
Egy Azure-hálózat toowhich csatlakozni fog a feladatátvételt követően létrehozott Azure virtuális gépeken van szüksége.

* hello hello hálózat legyen ugyanabban a régióban, hello Recovery Services-tároló.
* Attól függően, hogy az erőforrás-modellje hello meg toouse az Azure virtuális gépek a feladatátvételt, beállított hello Azure hálózati [Resource Manager módra](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) vagy [klasszikus módban](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
* Javasoljuk, hogy a kezdés előtt hozza létre a hálózatot. Ha ezt elmulasztja, toodo kell azt a Site Recovery üzembe helyezése során.
A Site Recovery által használt Azure-hálózatok nem lehet [áthelyezése](../azure-resource-manager/resource-group-move-resources.md) belül hello azonos, vagy másik, előfizetések között.

### <a name="set-up-an-azure-storage-account"></a>Azure-tárfiók beállítása
* A standard vagy prémium replikálja tooAzure toohold adatok Azure storage-fiók szükséges. [Prémium szintű storage](../storage/common/storage-premium-storage.md) szolgál egy folyamatosan magas I/O teljesítménye és kis késleltetésű toohost IO-igényes munkaterhelések igénylő virtuális gépektől. Ha azt szeretné, hogy toouse egy prémium szintű fiók toostore replikált adatokból, is szükség van egy standard tárolási fiók toostore replikálási naplókhoz, hogy a folyamatban lévő rögzítési tooon helyszíni adatokhoz módosítja. hello fióknak kell lennie a hello hello és ugyanabban a régióban Recovery Services-tároló.
* Attól függően, hogy az erőforrás-modellje hello meg toouse az Azure virtuális gépek a feladatátvételt, a fiók beállítása [Resource Manager módra](../storage/common/storage-create-storage-account.md) vagy [klasszikus módban](../storage/common/storage-create-storage-account.md).
* Javasoljuk, hogy még a kezdés előtt állítsa be a fiókot. Ha ezt elmulasztja, toodo kell azt a Site Recovery üzembe helyezése során.
- Vegye figyelembe, hogy a Site Recovery által használható storage-fiókok nem lehet [áthelyezése](../azure-resource-manager/resource-group-move-resources.md) belül hello azonos, vagy másik, előfizetések között.

### <a name="prepare-hello-vmm-server"></a>Hello VMM-kiszolgáló előkészítése
* Győződjön meg arról, hogy hello VMM-kiszolgáló megfelel-e hello [Előfeltételek](#prerequisites).
* Site Recovery üzembe helyezése során megadhatja, hogy a VMM-kiszolgálón futó összes felhő elérhető hello Azure-portálon kell lennie. Ha csak bizonyos felhők tooappear hello portálon, engedélyezheti ezt a beállítást hello felhő hello VMM felügyeleti konzolján.

### <a name="prepare-for-network-mapping"></a>A hálózatleképezés előkészítése
A Site Recovery üzembe helyezésekor a hálózatleképezést is be kell állítania. Hálózatleképezés kapcsolatot hoz létre a VMM Virtuálisgép-hálózatok és cél Azure-hálózatok, a következő tooenable hello adatforrás között:

* Feladatok átadása a hello azonos gépek hálózati is más tooeach csatlakozni, akkor is, ha azok még nem a feladatátvételt a hello azonos módon vagy hello azonos helyreállítási tervben.
* Ha a hálózati átjáró hello cél Azure-hálózat van beállítva, az Azure virtuális gépek tooon helyszíni virtuális gépek is elérheti.
* tooset fel a hálózatra való leképezést, ide van mire van szüksége:

  * Győződjön meg arról, hogy hello forrás Hyper-V gazdakiszolgálón futó virtuális gépek csatlakoztatott tooa VMM Virtuálisgép-hálózatot. Erre a hálózatra kell csatolt tooa hello felhő társított logikai hálózat.
  * Rendelkezzen egy, a [fentiekben](#set-up-an-azure-network) leírt Azure-hálózattal.

## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson az **Új** > **Felügyelet és kezelés** > **Backup és Site Recovery (OMS)** lehetőségre.

    ![Új tároló](./media/site-recovery-vmm-to-azure/new-vault3.png)
3. A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban. Ha egynél több előfizetéssel rendelkezik, válasszon egyet ezek közül.
4. [Hozzon létre egy erőforráscsoportot](../azure-resource-manager/resource-group-template-deploy-portal.md), vagy válasszon ki egy meglévőt. Válassza ki a kívánt Azure-régiót. Gépek replikált toothis terület lesz. támogatott toocheck régiók, tekintse meg a cikknek a földrajzi elérhetőséggel [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/)
5. Ha azt szeretné, hogy tooquickly hozzáférés hello tároló a hello irányítópult, kattintson a **PIN-kód toodashboard** > **tároló létrehozása**.

    ![Új tároló](./media/site-recovery-vmm-to-azure/new-vault.png)

hello új tároló megjelenik hello **irányítópult** > **összes erőforrás**, és a fő hello **Recovery Services-tárolók** panelen.


## <a name="select-hello-protection-goal"></a>Hello védelmi cél kiválasztása

Jelölje ki a tooreplicate, és a tooreplicate, ahová.

1. A **Recovery Services-tárolók**, jelölje be hello tárolóban.
2. A **Bevezetés** ablakban kattintson a **Site Recovery** > **Az infrastruktúra előkészítése** > **Védelmi cél** lehetőségre.

    ![Célok megválasztása](./media/site-recovery-vmm-to-azure/choose-goals.png)
3. A **védelmi cél** válasszon **tooAzure**, és válassza ki **Igen, a Hyper-V-vel**. Válassza ki **Igen** tooconfirm VMM toomanage Hyper-V-gazdagépek és a helyreállítási hely hello használata. Ezután kattintson az **OK** gombra.

## <a name="set-up-hello-source-environment"></a>Hello forráskörnyezet beállítása

Hello Azure Site Recovery Provider telepítése hello VMM-kiszolgálón, és regisztrálja hello kiszolgálót hello tárolóban. Hello Azure Recovery Services agent telepítése a Hyper-V gazdagépeken.

1. Kattintson **Az infrastruktúra előkészítése** > **Forrás** lehetőségre.

    ![A forrás beállítása](./media/site-recovery-vmm-to-azure/set-source1.png)

2. A **forrás előkészítése**, kattintson a **+ VMM** tooadd egy VMM-kiszolgáló.

    ![A forrás beállítása](./media/site-recovery-vmm-to-azure/set-source2.png)

3. A **kiszolgáló hozzáadása**, ellenőrizze, hogy **System Center VMM-kiszolgáló** megjelenik **kiszolgálótípus** és, hogy hello VMM-kiszolgáló megfelel-e hello [Előfeltételek és URL-címe követelmények](#prerequisites).
4. Töltse le a hello Azure Site Recovery Provider telepítőfájlját.
5. Hello regisztrációs kulcs letöltése. Erre a telepítő futtatása során lesz szükség. hello kulcs a generálásától öt napig esetén érvényes.

    ![A forrás beállítása](./media/site-recovery-vmm-to-azure/set-source3.png)


## <a name="install-hello-provider-on-hello-vmm-server"></a>Telepítse a szolgáltató hello hello VMM-kiszolgálón

1. Futtassa a szolgáltató telepítőfájl hello hello VMM-kiszolgálón.
2. A **Microsoft Update** lapon kérheti a frissítések beszerzését, így a rendszer a Microsoft Update-szabályzatnak megfelelően telepíteni fogja a Providerhez kiadott frissítéseket.
3. A **telepítési**, fogadja el vagy módosítsa a hello Provider alapértelmezett telepítési helyét, majd kattintson **telepítése**.

    ![Telepítés helye](./media/site-recovery-vmm-to-azure/provider2.png)
4. Kattintson a telepítés befejezése után **regisztrálása** tooregister hello VMM-kiszolgáló hello tárolóban lévő állapottal.
5. A hello **tároló beállításait** kattintson **Tallózás** tooselect hello tároló kulcsfájlját. Adja meg a hello Azure Site Recovery-előfizetést és hello tároló nevére.

    ![Kiszolgáló regisztrációja](./media/site-recovery-vmm-to-azure/provider10.PNG)
6. A **internetkapcsolat**, adja meg, hogyan hello VMM-kiszolgálón futó Provider keresztül fog csatlakozni tooSite helyreállítási hello hello internet.

   * Ha közvetlenül hello szolgáltató tooconnect, jelölje be **csatlakozzon közvetlenül a Site Recovery tooAzure proxy nélkül**.
   * Ha az aktuálisan használt proxy hitelesítést igényel, vagy toouse egyéni proxyt szeretne, válassza ki a **csatlakozzon a Site Recovery proxykiszolgálóval tooAzure**.
   * Ha egyéni proxyt használ, adja meg a hello cím, port és hitelesítő adatait.
   * Ha a proxy használata esetén kell már engedélyezte a hello URL-címeket a [Előfeltételek](#on-premises-prerequisites).
   * Ha egyéni proxyt használ, a VMM RunAs-fiókot (DRAProxyAccount) használatával jön létre automatikusan a megadott hello proxy hitelesítő adatokat. Hello-proxy kiszolgáló konfigurálása, így a fiók elvégezhesse a hitelesítést. hello VMM RunAs-fiók beállításait hello VMM-konzolon módosíthatja. A **beállítások**, bontsa ki a **biztonsági** > **futtató fiókok**, majd módosítsa a draproxyaccount jelszavát hello jelszót. Szüksége lesz toorestart hello VMM szolgáltatást, hogy ez a beállítás lép érvénybe.

     ![internet](./media/site-recovery-vmm-to-azure/provider13.PNG)
7. Fogadja el, vagy módosítsa az adatok titkosításához automatikusan létrehozott SSL-tanúsítvány hello helyét. Ez a tanúsítvány akkor használatos, ha engedélyezi az adattitkosítást a hello Azure Site Recovery portálon az Azure által védett felhő. Őrizze biztonságos helyen a tanúsítványt. A feladatátvételi tooAzure futtatásakor szüksége lehet rájuk toodecrypt, ha engedélyezett az adattitkosítás.

    > [!NOTE]
    > Ajánlott toouse hello meghajtótitkosítási képességet hello adattitkosítási Azure Site Recovery által megadott helyett, inaktív adatok titkosítása az Azure által biztosított. Azure által biztosított hello meghajtótitkosítási képességet is be kell kapcsolni egy tárolási > fiókot, és segít a teljesítmény, az Azure storage hello titkosítási/visszafejtési kezeli.
    > [További információk a Storage szolgáltatás Azure által biztosított titkosításáról](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption).
    
8. A **kiszolgálónév**, adjon meg egy rövid nevet tooidentify hello VMM-kiszolgáló hello tárolóban lévő állapottal. Fürtkonfiguráció adja meg a hello VMM-fürtszerepkör nevét.
9. Engedélyezése **felhőmetaadatok szinkronizálása**, ha azt szeretné, toosynchronize metaadatokat VMM-kiszolgálón hello hello tárolóban futó összes felhő. Ez a művelet csak egyszer minden kiszolgálón toohappen van szüksége. Ha nem szeretné, toosynchronize összes felhő, hagyja bejelölve ezt a beállítást, és szinkronizálja egyenként a hello felhőtulajdonságok hello VMM-konzolon. Kattintson a **regisztrálása** toocomplete hello folyamat.

    ![Kiszolgáló regisztrációja](./media/site-recovery-vmm-to-azure/provider16.PNG)
10. Elindul a regisztráció. Regisztráció befejezése után hello kiszolgáló megjelenik **Site Recovery-infrastruktúra** > **VMM-kiszolgáló**.


## <a name="install-hello-azure-recovery-services-agent-on-hyper-v-hosts"></a>Hello Azure Recovery Services agent telepítése a Hyper-V gazdagépek

1. Hello szolgáltató beállítása után szüksége toodownload hello telepítőfájl hello Azure Recovery Services Agent ügynököt. Futtassa a telepítőt a VMM-felhő hello minden Hyper-V kiszolgálón.

    ![Hyper-V helyek](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)
2. Az **Előfeltételek ellenőrzése** részben kattintson a **Tovább** gombra. A rendszer automatikusan telepíti a hiányzó előfeltételeket.

    ![Recovery Services Agent, előfeltételek](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)
3. A **telepítési beállítások**, fogadja el, vagy módosítsa a hello telepítési helyét, és hello gyorsítótár helyét. Hello gyorsítótár konfigurálhat egy meghajtón, amelyen legalább 5 GB tárterület érhető el, de azt javasoljuk, hogy a gyorsítótárazáshoz használt lemezen legalább 600 GB szabad terület. Ezt követően kattintson a **Telepítés** gombra.
4. Kattintson a telepítés befejezése után **Bezárás** toofinish.

    ![A MARS Agent regisztrálása](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

### <a name="command-line-installation"></a>Parancssori telepítés
Hello Microsoft Azure Recovery Services Agent a parancssorból a következő parancs hello segítségével telepítheti:

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-toosite-recovery-from-hyper-v-hosts"></a>Internetes proxy hozzáférés tooSite helyreállítási Hyper-V gazdagépekről beállítása

hello Recovery Services Agent ügynököt a Hyper-V gazdagépeken futó virtuális gép replikációs internet access tooAzure igényeinek. Ha elérni hello internet egy proxyn keresztül, állítsa be az alábbiak szerint:

1. Hello Microsoft Azure Backup szolgáltatás MMC beépülő modul megnyitásához hello Hyper-V gazdagépen. Alapértelmezés szerint a Microsoft Azure Backup parancsikonja áll rendelkezésre, hello asztalon vagy a C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Kattintson a beépülő modul hello **tulajdonságainak módosítása**.
3. A hello **proxykonfigurációt** lapra, adja meg a proxykiszolgáló-adatok.

    ![A MARS Agent regisztrálása](./media/site-recovery-vmm-to-azure/mars-proxy.png)
4. Ellenőrizze, hogy hello ügynök képes elérni hello URL-címeket a hello [Előfeltételek](#on-premises-prerequisites).

## <a name="set-up-hello-target-environment"></a>Hello célkörnyezet beállítása
Adja meg a replikáláshoz használt hello az Azure storage-fiók toobe, és hello Azure hálózati toowhich Azure virtuális gépek a feladatátvételt követően csatlakozhatnak.

1. Kattintson a **infrastruktúra előkészítése** > **cél**válassza ki a hello előfizetés, ahová átadja a virtuális gépek toocreate hello erőforráscsoport hello. Válassza ki a beállítani kívánt toouse (klasszikus vagy erőforrás-kezelés) Azure virtuális gépek a feladatátvételt hello hello telepítési modell.

    ![Storage](./media/site-recovery-vmm-to-azure/enablerep3.png)

2. A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.

    ![Storage](./media/site-recovery-vmm-to-azure/compatible-storage.png)

3. Ha még nem hozott létre egy tárfiókot, és azt szeretné, hogy egy toocreate erőforrás-kezelő használatával kattintson **+ tárfiók** toodo megtenni.  A hello **storage-fiók létrehozása** panelen adja meg egy fiók nevét, típusát, előfizetés és hely. hello fióknak kell lennie a hello hello és ugyanazon a helyen Recovery Services-tároló.

   ![Storage](./media/site-recovery-vmm-to-azure/gs-createstorage.png)


   * Ha azt szeretné, toocreate hello klasszikus modellt használó tárfiókot, ehhez a hello Azure-portálon. [További információ](../storage/common/storage-create-storage-account.md)
   * A replikált adatok használata a prémium szintű storage-fiók, állítsa be egy további standard szintű tárfiók toostore replikálási naplókhoz, folyamatban lévő változtatások tooon helyszíni adatok rögzítéséhez.
5. Ha még nem hozott létre az Azure-hálózatot, és azt szeretné, hogy egy toocreate erőforrás-kezelő használatával kattintson **+ hálózat** toodo megtenni. A hello **virtuális hálózat létrehozása** panelen adja meg a hálózati név, címtartomány, alhálózati adatokat, előfizetést és helyet. hello hálózat legyen hello hello és ugyanazon a helyen Recovery Services-tároló.

   ![Network (Hálózat)](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

   Ha azt szeretné, toocreate hello klasszikus modellt használó hálózatot, ehhez a hello Azure-portálon. [További információk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Hálózatleképezés konfigurálása

* [Itt elolvashatja](#prepare-for-network-mapping) a hálózatleképezés működésének rövid ismertetését.
* Győződjön meg arról, hogy virtuális gépek a VMM-kiszolgálón hello csatlakoztatott tooa Virtuálisgép-hálózatot, és, hogy létrehozta-e legalább egy Azure virtuális hálózat. Több Virtuálisgép-hálózatot csatlakoztatott tooa egyetlen Azure-hálózat lehet.

Konfigurálja az alábbiak szerint a leképezéseket:

1. A **Site Recovery-infrastruktúra** > **Hálózatleképezések** > **Hálózatleképezés**, kattintson a hello **+ Hálózatleképezés**  ikonra.

    ![Hálózatleképezés](./media/site-recovery-vmm-to-azure/network-mapping1.png)
2. A **hálózatleképezés hozzáadása**, jelölje be hello forrás VMM-kiszolgálón, és **Azure** hello célként.
3. Ellenőrizze a hello előfizetés és hello telepítési modell a feladatátvételt követően.
4. A **Forráshálózat**, jelölje be hello forrás helyszíni Virtuálisgép-hálózat kívánt toomap hello VMM-kiszolgálóhoz társított hello listából.
5. A **célhálózat**, válassza ki hello Azure hálózati melyik replika Azure virtuális gépeken található során jönnek létre. Ezután kattintson az **OK** gombra.

    ![Hálózatleképezés](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Ez történik a hálózatleképezés megkezdésekor:

* Hello forrás Virtuálisgép-hálózat meglévő virtuális gépekhez csatlakoztatott toohello célhálózat is, ha a leképezés kezdődik. Új virtuális gépek csatlakoztatott toohello forrás Virtuálisgép-hálózathoz kapcsolódó replikáláskor toohello leképezve az Azure-hálózatot.
* Meglévő hálózatleképezés módosítása esetén a replika virtuális gépek új hello-beállítások használatával csatlakozzon.
* Ha hello célhálózatban már több alhálózat működik, és ezek egyikének rendelkezik hello neve megegyezik a virtual machine hello forrás alhálózati található, akkor hello replika virtuális gép a feladatátvételt követően toothat cél alhálózathoz csatlakozik.
* Ha nincsenek egyező nevű cél alhálózathoz, hello virtuálisgép kapcsolódik toohello hello hálózat első alhálózatát.

## <a name="configure-replication-settings"></a>Replikációs beállítások konfigurálása
1. Kattintson egy új replikációs házirendet toocreate **infrastruktúra előkészítése** > **replikációs beállítások** > **+ létrehozás és társítás**.

    ![Network (Hálózat)](./media/site-recovery-vmm-to-azure/gs-replication.png)
2. A **Házirend létrehozása és társítása** beállításnál adja meg a szabályzat nevét.
3. A **másolás gyakorisága**, adja meg, milyen gyakran tooreplicate különbözeti adatokat hello kezdeti replikálás (30 másodperces, 5 vagy 15 perc) után.

    > [!NOTE]
    >  Az 30 második gyakoriságát toopremium tárolási replikálása esetén nem támogatott. hello korlátozás hello pillanatképek számát / (100) blob támogatja prémium szintű storage határozza meg. [További információ](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. A **helyreállításipont-megőrzést**, adja meg, órákban mennyi ideig hello adatmegőrzési időtartam fogja az egyes helyreállítási pontok lehet. Védett gépek lehet helyreállítani egy időszakban tooany pont.
5. Az **Alkalmazáskonzisztens pillanatkép gyakorisága** beállítás azt határozza meg, hogy milyen gyakran hozzon létre a rendszer alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontokat (a beállítás értéke 1 és 12 óra között változhat). Hyper-V két különböző használ – standard pillanatkép, amely lefedő növekményes pillanatképet hello teljes virtuális gép, és egy alkalmazáskonzisztens pillanatkép, amely időpontban pillanatképet készít a hello alkalmazásadatok hello virtuális gépen belül. Alkalmazáskonzisztens pillanatképek a kötet árnyékmásolata szolgáltatás (VSS) tooensure, amely az alkalmazások konzisztens állapotban legyenek, hello pillanatkép készítésének időpontjában. Vegye figyelembe, hogy ha engedélyezi az alkalmazáskonzisztens pillanatképeket, az hatással hello forrás virtuális gépeken futó alkalmazások teljesítményére. Győződjön meg arról, hogy hello beállított érték kisebb, mint hello további helyreállítási pontok száma.
6. A **kezdeti replikáció kezdési ideje**, azt jelzi, ha toostart hello kezdeti replikálása. hello replikálást, az internetes sávszélességet, ezért érdemes tooschedule azt a foglalt munkaidőn kívül.
7. A **az Azure-on tárolt adatok titkosítása**, adja meg, hogy tooencrypt elhelyezett inaktív adatokat az Azure-tárfiókba. Ezután kattintson az **OK** gombra.

    ![Replikációs szabályzat](./media/site-recovery-vmm-to-azure/gs-replication2.png)
8. Ha egy új házirendet hoz létre, automatikusan rendelkezik hello VMM-felhő társítva. Kattintson az **OK** gombra. További VMM-felhőkben (és a bennük foglalt virtuális gépek hello) társíthatja a replikációs házirendet a **replikációs** > szabályzat neve > **VMM-felhő társítása**.

    ![Replikációs szabályzat](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="capacity-planning"></a>Kapacitástervezés

Most, hogy az alapszintű infrastruktúra készen áll, fogjon hozzá a kapacitás megtervezéséhez, és döntse el, hogy szüksége van-e további erőforrásokra.

A Site Recovery biztosít a kapacitás planner toohelp hello erőforrások osszon ki a forrás környezetében, a Site Recovery-összetevőkhöz, a hálózat és a tároló számára. A virtuális gépek, a lemezek és a tárolás átlagos számán alapuló becsléseket gyors módját, vagy a részletes módot, amelyben bemeneti adatok hello munkaterhelés szinten hello planner futtathatja. Előkészületek:

* Gyűjtse össze a replikálási környezetre vonatkozó adatokat, többek között a virtuális gépeket, a virtuális gépekhez tartozó lemezeket, valamint a lemezenkénti tárterületeket.
* Hello konfigurálnia kell a replikált adatok napi adatváltozás (forgalom) arány becslése. Használhatja a hello [Capacity planner a Hyper-V replika](https://www.microsoft.com/download/details.aspx?id=39057) toohelp ehhez.

1. Kattintson a **letöltése**, toodownload hello eszközt, és futtassa. [A cikk elolvasása hello](site-recovery-capacity-planner.md) , amely kísérik hello eszköz.
2. Amikor elkészült, válassza ki a **Igen** a **futtatta hello Capacity Planner**?

   ![Kapacitástervezés](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

   További információk [a hálózati sávszélesség szabályozásáról](#network-bandwidth-considerations)




## <a name="enable-replication"></a>A replikáció engedélyezése

Mielőtt elkezdené, győződjön meg arról, hogy az Azure felhasználói fiók rendelkezik-e a szükséges hello [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) egy új virtuális gép tooAzure tooenable replikációját.

Most már engedélyezheti a replikációt a következők szerint:

1. Kattintson **2. lépés: Az alkalmazás replikálása** > **Forrás** elemre. Hello replikáció első alkalommal engedélyezését, kattintson a **+ replikálás** a hello tároló tooenable a további gépek replikációjának.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/enable-replication1.png)
2. A hello **forrás** panelen, jelölje be hello VMM-kiszolgáló és hello felhő, mely hello Hyper-V gazdagépek találhatók. Ezután kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/enable-replication-source.png)
3. A **cél**hello előfizetés, a feladatátvételt követően telepítési modell, válassza ki, és hello replikált adatok tárolására használni kívánt tárfiókot.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/enable-replication-target.png)
4. Válassza ki a kívánt toouse hello tárfiókot. Ha azt szeretné, hogy egy másik tárolóhelyre fiókhoz, mint a toouse, akkor [hozzon létre egyet](#set-up-an-azure-storage-account). Ha a replikált adatok használata a prémium szintű storage-fiók esetén kell tooselect egy további standard tárolási fiók toostore replikációs naplók rögzítése folyamatban lévő változtatások tooon helyszíni data.toocreate hello Resource Manager modellt használó tárfiókot Kattintson a **hozzon létre új**. Ha azt szeretné, toocreate hello klasszikus modellt használó tárfiókot, ehhez [hello Azure-portálon a](../storage/common/storage-create-storage-account.md). Ezután kattintson az **OK** gombra.
5. SELECT hello Azure hálózati és alhálózati toowhich Azure virtuális gépeken fog csatlakozni, ha a feladatátvételt követően jönnek létre. Válassza ki **beállítás most a kijelölt gépekhez**, tooapply hello beállítás tooall számítógépek választja a védelem. Válassza ki **konfigurálását később**, tooselect hello gépenként Azure-hálózatot. Ha azt szeretné, hogy toouse azokat van egy másik hálózaton, akkor [hozzon létre egyet](#set-up-an-azure-network). egy toocreate hello Resource Manager modellt kattintson **hozzon létre új**. Ha azt szeretné, toocreate hello klasszikus modellt használó hálózatot, ehhez [hello Azure-portálon a](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Ha szükséges, válassza ki az alhálózatot. Ezután kattintson az **OK** gombra.
6. A **virtuális gépek** > **válassza ki a virtuális gépek**kattintson, és válassza az egyes gépek tooreplicate szeretné. Csak olyan gépeket választhat, amelyeken használható a replikáció funkció. Végül kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/enable-replication5.png)

7. A **tulajdonságok** > **tulajdonságainak konfigurálása**hello operációs rendszert válasszon ki a kijelölt hello virtuális gépek és operációsrendszer-lemez hello.

    - Győződjön meg arról, hogy hello Azure virtuális gép nevét (cél neve) megfelel [Azure virtuális gép követelményeinek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).   
    - Alapértelmezés szerint az összes hello lemez a virtuális gép hello replikációhoz vannak kijelölve, de a lemezek tooexclude törölheti őket.

        - Érdemes lehet tooexclude lemezek tooreduce replikáció sávszélesség. Például előfordulhat, hogy nem kívánja tooreplicate lemezek ideiglenes adatokkal, vagy adatokat, minden alkalommal, amikor frissítette a gépek vagy alkalmazások (például pagefile.sys vagy Microsoft SQL Server tempdb) újraindul. Unselecting hello lemez hello lemez lehet kizárni a replikálásból.
        - Csak az alaplemezek zárhatók ki. Operációsrendszer-lemezek nem zárhatók ki.
        - Azt javasoljuk, hogy ne zárja ki a dinamikus lemezeket. A Site Recovery nem tudja azonosítani, hogy egy virtuális merevlemez a vendég virtuális gépben alap- vagy dinamikus lemez-e. Ha az összes függő dinamikus kötet lemez nem zárható ki, hello védett dinamikus lemez állapotúként lesz megjelölve, a hibás lemez hello VM átadja a feladatokat, és a lemezen levő hello adatok nem lesz elérhető.
        - A replikáció engedélyezése után már nem lehet ahhoz lemezeket hozzáadni, vagy lemezeket eltávolítani belőle. Ha szeretné, hogy tooadd, vagy zárja ki egy lemezt, toodisable védelmi hello VM van szükség, és majd engedélyezze újra.
        - Az Azure-ban manuálisan létrehozott lemezek nem vesznek részt a feladatátvételben. Például ha több mint három lemezt sikertelen, és hozzon létre két közvetlenül az Azure virtuális Gépen, csak hello három lemezeket, amelyeket a feladatátvételt volt sikertelen lesz újból az Azure tooHyper-V. Feladat-visszavétel, vagy a fordított replikálást a Hyper-V tooAzure manuálisan létrehozott lemezek nem adhat meg.
        - Ha kizárja a olyan lemezzel, amelyet egy alkalmazás toooperate van szükség, miután feladatátvételi tooAzure kell toocreate manuálisan az Azure, így az adott hello replikálva alkalmazás futtatható. Azt is megteheti az Azure automation sikerült integrálja a helyreállítási terv toocreate hello lemez hello gép feladatátvétele során.

    Kattintson a **OK** toosave módosításokat. A további tulajdonságokat később is beállíthatja.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)

8. A **replikációs beállítások** > **replikáció beállításainak konfigurálása**, válassza ki a kívánt tooapply hello védett virtuális gépek hello replikációs házirend. Ezután kattintson az **OK** gombra. Módosíthatja a hello replikációs házirendet a **replikációs házirendek** > szabályzat neve > **beállításainak szerkesztése**. Az itt megadott módosítások a már replikálás alatt álló, illetve az újonnan hozzáadott gépekre egyaránt érvényesek.

   ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/enable-replication7.png)

Hello állapotának nyomon követheti **Védelemengedélyezési** feladat **feladatok** > **Site Recovery-feladatok**. Hello után **Védelemvéglegesítési** feladat fut, hello gép készen áll a feladatátvételre.

### <a name="view-and-manage-vm-properties"></a>A virtuális gépek tulajdonságainak megtekintése és kezelése

Azt javasoljuk, hogy ellenőrizze a hello forrásgép hello tulajdonságait. Ne feledje, hogy hello Azure virtuális gép nevének meg kell felelnie az [Azure virtuális gép követelményeinek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. A **védett elemek**, kattintson a **replikált elemek**, és válasszon hello gép toosee hozzá tartozó részletek.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/vm-essentials.png)
2. A **tulajdonságok**, megtekintheti a replikációs és feladatátvételi adatait hello virtuális gép.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/test-failover2.png)
3. A **számítás és hálózat** > **számítási tulajdonságok**, megadhatja a hello Azure virtuális gép nevét és a cél méretét. Hello neve toocomply rendelkező módosítása [Azure-követelményeknek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) Ha kell. Is megtekintheti és hello IP-címet, amely hozzá van rendelve, hello cél hálózati és alhálózati vonatkozó információk toohello Azure virtuális Gépen.
Vegye figyelembe:

   * Beállíthat hello cél IP-címet. Ha nem ad meg egy címet, a gép a feladatátvételt hello DHCP fogja használni. Ha egy címet, amely nem használható feladatátadásra, hello feladatátvétel sikertelen lesz. cél IP-cím a feladatátvételi teszthez használható, ha hello címkészletében nem áll rendelkezésre a feladatátvételi teszt hálózatában hello hello.
   * hálózati adapterek száma hello hello mérete határozza meg hello cél virtuális gépre, az alábbiak szerint határozza meg:

     * Ha hálózati adapterek hello forrásgépen hello száma kisebb vagy egyenlő, mint hello cél gép méretéhez engedélyezett adapterek toohello számát, majd hello cél lesz hello ugyanannyi adapter hello forrásaként.
     * Ha hello hello forrás virtuális gép adaptereinek száma meghaladja a hello cél méretéhez engedélyezett hello számát, majd hello cél maximális szolgál.
     * Ha például a forrásgépen két hálózati adapterrel és hello célgép mérete négy támogatja, hello célgépen két adapter fog működni. Ha hello forrásgépen két adapter, de hello támogatott célméretet csak akkor támogatja a egyet, majd hello célgépen csak egy adapter fog működni.     
     * Ha hello virtuális gép több hálózati adapterrel rendelkezik, azok fog összes kapcsolódás toohello ugyanazon a hálózaton.

     ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/test-failover4.png)

4. A **lemezek** hello replikálandó virtuális gép hello operációsrendszer- és adatlemezek megtekintheti.

#### <a name="managed-disks"></a>Felügyelt lemezek

A **számítás és hálózat** > **számítási tulajdonságok**, ha azt szeretné, hogy tooattach felügyelt lemezek tooyour gép áttelepítési tooAzure "Az kezelt lemezek" beállítás túl "Yes" a virtuális gép hello állíthatja be. Kezelt lemezeken Azure IaaS virtuális gépek Lemezkezelés leegyszerűsíti hello virtuális gépek lemezei társított hello storage-fiókok kezelése. [További információk a felügyelt lemezekről](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview).

   - Felügyelt lemezek létrehozott és csatolt toohello virtuális gép csak egy feladatátvételi tooAzure. A védelem engedélyezése után a helyszíni gépeket adatokat továbbra is tooreplicate toostorage fiókok.
   Felügyelt lemezek csak hello Resource manager üzembe helyezési modellben használatával telepített virtuális gépeket hozható létre.  

  > [!NOTE]
  > Feladat-visszavétel Azure tooon helyszíni Hyper-V környezetben jelenleg nem támogatott a felügyelt lemezek gépek. Állítsa be "Az kezelt lemezek" túl "Yes" csak akkor, ha azt tervezi, toomigrate ezen a számítógépen az Azure-bA.

   - Ha úgy állítja be "Az kezelt lemezek" túl "Yes", csak rendelkezésre állási készletek hello erőforráscsoportban "Az kezelt lemezek" készlettel túl "Yes" lenne kijelölésnél érhetők el. Ennek az az oka felügyelt lemezzel rendelkező virtuális gépek csak része lehet "Kezelt lemez" tulajdonságkészlet rendelkezésre állási készletek túl "Yes". Győződjön meg arról, hogy hozzon létre rendelkezésre állási csoportok "Kezelt lemez" tulajdonság beállítva a felügyelt leképezési toouse lemezek a feladatátvételi alapján.  Hasonlóképpen ha úgy állítja be "Az kezelt lemezek" túl "nem", csak rendelkezésre állási készletek hello erőforráscsoportban, "Az kezelt lemezek" tulajdonsága túl "nem" lenne kijelölésnél érhetők el. [További információk a felügyelt lemezekről és a rendelkezésre állási csoportokról](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Ha replikációhoz használt hello tárfiók volt titkosítva Storage szolgáltatás titkosítási bármikor időben, a felügyelt lemezek feladatátvétel során sikertelen lesz. Akkor is, vagy állítsa be "Az kezelt lemezek" túl "nem", és ismételje meg a feladatátvételi vagy tiltsa le a védelmet a virtuális gép hello és a védelmét, tooa storage-fiók, amelynek nincs Storage szolgáltatás titkosítási bármikor időben engedélyezve van.
  > [További információk a Storage Service Encryptionről és a felügyelt lemezekről](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="test-hello-deployment"></a>Hello központi telepítés tesztelése

tootest hello telepítési futtathatja egyetlen virtuális gép vagy egy vagy több virtuális gépet tartalmazó helyreállítási terv feladatátvételi tesztjét.

### <a name="before-you-start"></a>Előkészületek

 - Ha azt szeretné, hogy a virtuális gépek tooconnect tooAzure, feladatátvételt követően RDP segítségével ismerje [tooconnect előkészítése](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - az Active Directory és a DNS-toocopy van szüksége a tesztkörnyezetben toofully tesztje. [További információk](site-recovery-active-directory.md#test-failover-considerations).

### <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása

1. egy virtuális, keresztül toofail a **replikált elemek**, kattintson a virtuális gép hello > **+ feladatátvételi teszt**.
2. a helyreállítás alatt toofail tervez, a **helyreállítási tervek**, kattintson a jobb gombbal hello terv > **feladatátvételi teszt**. a helyreállítási terv toocreate [kövesse ezeket az utasításokat](site-recovery-create-recovery-plans.md).
3. A **feladatátvételi teszt**, jelölje be hello Azure virtuális gépek Azure-hálózat toowhich csatlakozzon a feladatátvételt követően.
4. Kattintson a **OK** toobegin hello feladatátvételi. Előrehaladásának parancsával hello VM tooopen tulajdonságát, vagy a hello **feladatátvételi teszt** feladat **Site Recovery-feladatok**.
5. Hello feladatátvétel befejezése után meg kell jelennie toosee hello replika Azure machine hello Azure-portálon jelenik meg > **virtuális gépek**. Győződjön meg arról, hogy a virtuális gép hello hello megfelelő méretűek, hogy az csatlakozott a megfelelő hálózati toohello, és fut-e.
6. Ha elvégezte a feladatátvételt követően, meg kell tudni tooconnect toohello Azure virtuális Gépen.
7. Ha elkészült, kattintson a **karbantartása a feladatátvételi teszt** a hello helyreállítási terv. A **megjegyzések** és hello feladatátvételi teszthez kapcsolatos megfigyelések feljegyzéséhez mentéséhez. Ezzel a lépéssel törli a teszt feladatátvétele során létrehozott hello virtuális gépek.

További tudnivalókért olvassa el a hello [teszt feladatátvételi tooAzure](site-recovery-test-failover-to-azure.md) cikk.

## <a name="monitor-hello-deployment"></a>A figyelő hello központi telepítés

Íme, hogyan figyelheti a konfigurációs beállításokat, és állapotának a Site Recovery üzembe helyezése hello:

1. Kattintson a hello tároló neve tooaccess hello **Essentials** irányítópult. Itt megtalálja a Site Recovery-feladatokat, a replikációs állapotot, a replikálási terveket, a kiszolgáló állapotát, valamint az eseményeket.  Testre szabható **Essentials** tooshow hello csempék és elrendezések, amelyek a leghasznosabb tooyou, többek között a más helyreállítás és biztonsági mentési tárolók hello állapotát.

    ![Alapvető erőforrások](./media/site-recovery-vmm-to-azure/essentials.png)
2. A **állapotfigyelő**, figyelheti a helyszíni kiszolgálók (VMM- vagy konfigurációs kiszolgálók) kapcsolatos problémákat, és hello keletkező Site Recovery által hello az elmúlt 24 órában.
3. A hello **replikált elemek**, **helyreállítási tervek**, és **Site Recovery-feladatok** csempék kezelheti és a replikálás. A feladatokat részletesen is megtekintheti a **Feladatok** > **Site Recovery-feladatok** menüpontban.

## <a name="command-line-installation-for-hello-azure-site-recovery-provider"></a>Parancssori hello Azure Site Recovery Provider telepítése

hello Azure Site Recovery Provider parancssori hello is telepíthető. Ezzel a módszerrel a Server Core for Windows Server 2012 R2 szolgáltató használt tooinstall hello lehet.

1. Töltse le a hello szolgáltató telepítési és a regisztrációs kulcs tooa mappát. A mappa lehet például a következő: C:\ASR.
2. Egy rendszergazda jogú parancssorból futtassa ezen parancsok tooextract hello Provider telepítőjét:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Ez a parancs tooinstall hello összetevők futtatása:

            C:\ASR> setupdr.exe /i
4. Ezután futtassa a parancsokat, tooregister hello server hello tárolóban:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>       

Az elemek magyarázata:

* **/ Credentials**: kötelező paraméter, amely megadja, hogy hol hello regisztrációs kulcsot tartalmazó fájlt található.  
* **/ FriendlyName**: hello Hyper-V gazdakiszolgáló hello Azure Site Recovery portálon megjelenő nevét hello paramétert kötelező megadni.
* * **/ EncryptionEnabled**: nem kötelező paraméter, ha a VMM-ben a Hyper-V virtuális gépeket replikál felhők tooAzure. Adja meg, ha tooencrypt virtuális gépek Azure-ban (a inaktív adatok titkosítása). Győződjön meg arról, hogy hello hello fájl neve egy **.pfx** bővítmény. A titkosítás alapértelmezés szerint ki van kapcsolva.

    > [!NOTE]
    > Ajánlott toouse hello meghajtótitkosítási képességet hello titkosítási beállítás (EncryptionEnabled) Azure Site Recovery által megadott helyett, inaktív adatok titkosítása az Azure által biztosított. Azure által biztosított hello meghajtótitkosítási képességet is be kell kapcsolni a tárfiók, és segítséget nyújt a titkosítási/visszafejtési hello Azure végezhető el, jobb teljesítmény érdekében  
    > végzi.
    > [További információk a Storage szolgáltatás Azure-beli titkosításáról](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption).
    
* **/ proxyaddress**: nem kötelező paraméter, amely hello hello proxykiszolgáló címét.
* **/ proxyport**: nem kötelező paraméter, amely hello proxykiszolgáló hello port.
* **/ proxyusername**: nem kötelező paraméter, amely hello proxy felhasználónevét határozza meg, (Ha a proxy hitelesítést igényel).
* **/ proxypassword**: nem kötelező paraméter meghatározza, hogy hello jelszó tooauthenticate proxykiszolgáló (ha hello proxy hitelesítést igényel).


### <a name="network-bandwidth-considerations"></a>Hálózati sávszélességgel kapcsolatos szempontok
Hello kapacitás planner eszköz toocalculate hello szükséges sávszélességet replikáció (a kezdeti replikálás, majd különbözeti) is használhatja. toocontrol hello replikációhoz felhasznált sávszélesség mértékének, közül néhány:

* **A sávszélesség szabályozását**: tooa másodlagos helyre replikált Hyper-V-forgalom halad át egy adott Hyper-V gazdagépen. Beállíthatja a gazdagép-kiszolgálón hello sávszélesség szabályozását.
* **Sávszélesség finomhangolása**: beállításkulcsok több replikációhoz használt hello sávszélességet is szabályozhatja.

#### <a name="throttle-bandwidth"></a>Sávszélesség szabályozása
1. Hello Microsoft Azure Backup szolgáltatás MMC beépülő modul megnyitásához hello Hyper-V gazdagép-kiszolgálón. Alapértelmezés szerint a Microsoft Azure Backup parancsikonja áll rendelkezésre, hello asztalon vagy a C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Kattintson a beépülő modul hello **tulajdonságainak módosítása**.
3. A hello **sávszélesség-szabályozási** lapon jelölje be **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél**, és állítsa be a munkaórákra hello és a munkaórákon kívüli időre. Érvényes tartomány: az 512 kbit/s too102 MB / s.

    ![Sávszélesség szabályozása](./media/site-recovery-vmm-to-azure/throttle2.png)

Is használhatja a hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) parancsmag tooset sávszélesség-szabályozás. Íme egy példa:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

A **Set-OBMachineSetting -NoThrottle** beállítás azt jelenti, hogy nincs szükség szabályozásra.

#### <a name="influence-network-bandwidth"></a>A hálózati sávszélesség szabályozása
Hello **UploadThreadsPerVM** beállításjegyzékbeli érték azt határozza meg hello egy lemezen adatátvitelre (kezdeti vagy változásreplikálásra replikációs) használt szálak száma. A nagyobb érték növeli a hello replikációhoz használt hálózati sávszélességet. Hello **DownloadThreadsPerVM** beállításazonosítót hello adatátvitel feladat-visszavétel során használt szálak számát határozza meg.

1. Hello beállításjegyzék, lépjen a túl**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.

   * Hello érték módosítása **UploadThreadsPerVM** (vagy hello kulcs létrehozása, ha még nem létezik) lemezek replikálásához használt toocontrol szálak számát.
   * Hello érték módosítása **DownloadThreadsPerVM** (vagy hello kulcs létrehozása, ha még nem létezik) az Azure-ból feladatátvételi forgalomhoz használt toocontrol szálak számát.
2. hello alapértelmezett értéke 4. "A szükségesnél több erőforrással" hálózatban ezek a kulcsok hello alapértelmezett értékekhez módosítani kell. hello maximális 32. Forgalom toooptimize hello érték figyelése.

## <a name="next-steps"></a>Következő lépések

Kezdeti replikálás befejeződik, és hello telepítés tesztelését, után hívhatja meg a feladatátvételek hello szükség szerint. [További](site-recovery-failover.md) feladatátvételek különböző típusú, és hogyan toorun őket.
