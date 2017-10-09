---
title: "VMware virtuális gépek tooAzure aaaReplicate |} Microsoft Docs"
description: "Összefoglalja azokat a VMware virtuális gépek tooAzure tárolási munkaterheléseinek replikálásához kell hello lépéseket"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: vmware-walkthrough-overview
ms.openlocfilehash: f800e7d8a3b59a86809a995748eacf87630a1713
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-virtual-machines-tooazure-with-site-recovery"></a>VMware virtuális gépek tooAzure a Site Recovery replikálása

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmware-to-azure.md)
> * [Klasszikus Azure portál](site-recovery-vmware-to-azure-classic.md)


Ez a cikk ismerteti, hogyan tooreplicate helyszíni VMware virtuális gépek tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

Ha azt szeretné, toomigrate VMware virtuális gépek tooAzure (csak feladatátvételi), olvasási [Ez a cikk](site-recovery-migrate-to-azure.md) további toolearn.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="deployment-steps"></a>A központi telepítés lépései

Mi szüksége toodo:

1. Előfeltételek és korlátozások ellenőrzése.
2. Azure-hálózat és tárfiókok beállítása.
3. Készítse elő hello a helyi számítógépen, amely azt szeretné, hogy toodeploy hello konfigurációs kiszolgálóval.
4. Készítse elő a VMware fiókok toobe a virtuális gépek automatikus felderítése, és szükség esetén hello mobilitási szolgáltatás leküldéses telepítéséhez.
4. Recovery Services-tároló létrehozása. hello tároló konfigurációs beállításait tartalmazza, és koordinálja a replikációt.
5. Adja meg a forrás, a cél és a replikációs beállításokat.
6. Hello mobilitási szolgáltatás telepítése virtuális gépeken tooreplicate szeretné.
7. Engedélyezze a hello virtuális gépek replikációját.
7. Futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik.

## <a name="prerequisites"></a>Előfeltételek

**Támogatási követelmények** | **Részletek**
--- | ---
**Azure** | További tudnivalók [Azure-követelmények](site-recovery-prereq.md#azure-requirements)
**Helyszíni konfigurációs kiszolgáló** | Windows Server 2012 R2 rendszerű VMware virtuális gép van szüksége, vagy később. Ez a kiszolgáló beállítása a Site Recovery üzembe helyezése során.<br/><br/> Alapértelmezett hello folyamat kiszolgáló és a fő célkiszolgáló is települnek a virtuális Gépet. Vertikális felskálázás, előfordulhat, hogy különálló folyamatkiszolgálót, és rendelkezik hello ugyanazok vonatkoznak, mint hello konfigurációs kiszolgáló.<br/><br/> További információ ezen összetevők [Itt](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)
**Helyszíni VMware-kiszolgálók** | Egy vagy több VMware vSphere, futtató kiszolgálók 6.0, 5.5, 5.1 legújabb frissítéseit. Kiszolgálók hello azonos hálózati hello konfigurációs kiszolgáló (vagy különálló folyamatkiszolgálót) kell elhelyezni.<br/><br/> Javasoljuk a vCenter server toomanage állomások, 6.0 vagy 5.5 rendszerű hello legújabb frissítéseit. Csak azok a szolgáltatások által biztosított 5.5 6.0-s verzió telepítésekor támogatottak.
**A helyszíni virtuális gépek** | Virtuális gépek kívánt tooreplicate rendszerűnek kell lennie egy [támogatott operációs rendszer](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), és adott a specifikációknak való [Azure-Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). Virtuális gép VMware-eszközök futó kell rendelkeznie.
**URL-címek** | meg kell hello konfigurációs kiszolgálót toothese URL hozzáférés:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Ha IP-címeken alapuló tűzfalszabályok szabály, győződjön meg arról, kommunikációs tooAzure lehetővé teszik.<br/></br> Hello engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443) port hello.<br/></br> Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).<br/><br/> Engedélyezi a hello MySQL letöltési URL-címe: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Mobilitási szolgáltatás** | Minden replikált virtuális gép telepítve.




## <a name="limitations"></a>Korlátozások

**A korlátozás** | **Részletek**
--- | ---
**Azure** | Tárolási és hálózati fiókoknak kell lenniük a hello és hello-tárolónak ugyanabban a régióban<br/><br/> A prémium szintű storage-fiókok használatához is szükség van egy standard fiók toostore replikálási naplók tárolásához<br/><br/> Közép-és Dél-Indiában toopremium fiókok nem replikálja.
**Helyszíni konfigurációs kiszolgáló** | VMware virtuális hálózatiadapter-típus VMXNET3 kell lennie. Ha nem, [a frissítés telepítése](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)<br/><br/> a vSphere PowerCLI 6.0 kell telepíteni.<br/><br> hello gépet nem lehet tartományvezérlő. hello gépnek rendelkeznie kell egy statikus IP-címet.<br/><br/> hello állomásnév 15 karakter lehet ne legyen, és az operációs rendszer angol nyelven kell lennie.
**VMware** | VCenter 6.0 csak 5.5 szolgáltatások támogatottak. A Site Recovery nem támogatja az új vCenter és vSphere 6.0 szolgáltatások például a kereszt-vCenter vMotion, a virtuális kötetek és a tárolási DRS.
**Virtuális gépek** | Győződjön meg arról [Azure virtuális gép korlátozásai](site-recovery-prereq.md#azure-requirements)<br/><br/> Titkosított lemezzel rendelkező virtuális gépeket, vagy az UEFI/EFI-rendszerbetöltés virtuális gépeken nem replikálja.<br/><br> A megosztott lemez nem használhatók. Ha hello forrás virtuális gép rendelkezik hálózati adapterek összevonása, azt konvertálja tooa egyetlen hálózati adapter a feladatátvételt követően.<br/><br/> Ha a virtuális gép iSCSI-lemez van, a Site Recovery átalakítja tooa VHD-fájlt a feladatátvételt követően. Hello iSCSI-tároló hello Azure virtuális gép elérhető, ha tooit csatlakozik, és azt és a virtuális merevlemez hello látja. Ha ez történik, válassza le a hello iSCSI-tároló.<br/><br/> Ha azt szeretné, hogy a virtuális Gépre kiterjedő konzisztencia tooenable, amely lehetővé teszi a munkaterhelés ugyanazon toobe helyre hello futó virtuális gépeket együtt tooa azonos adatokat mutasson, nyissa meg a virtuális gép hello 20004 portot.<br/><br/> Windows hello C meghajtó telepítve kell lennie. az operációsrendszer-lemez hello alapvető, és nem dinamikus kell lennie. hello adatlemez dinamikus lehet.<br/><br/> Linux/Etc/Hosts fájlt a virtuális gépek bejegyzéseit, amelyek hozzárendeléséről hello helyi állomás neve tooIP minden hálózati adapterekkel társított kell tartalmaznia. állomásnév, csatlakoztatási pontokat, eszköz neve, elérési utak és fájlnevek hello (/ etc; / usr) kell lennie csak angol nyelven.<br/><br/> Adott típusú [Linux tárolási](site-recovery-support-matrix-to-azure.md#support-for-storage) támogatottak.<br/><br/>Hozzon létre, vagy állítsa be **disk.enableUUID=true** hello virtuális gép beállításai. Egységes UUID toohello VMDK, ez biztosítja, így a megfelelő csatlakoztatja, és biztosítja, hogy az csak a változási különbözeteket átvitt hátsó tooon helyszíni feladat-visszavétel, anélkül, hogy a teljes replikáció során.

## <a name="set-up-azure"></a>Azure beállítása

1. [Állítsa be az Azure-hálózatot](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).
    - Azure virtuális gépek kerülnek ehhez a hálózathoz, ha a feladatátvételt követően jönnek létre.
    - Beállíthatja a hálózat [erőforrás-kezelő](../resource-manager-deployment-model.md), vagy a klasszikus mód.

2. Állítson be egy [Azure storage-fiók](../storage/storage-create-storage-account.md#create-a-storage-account) a replikált adatok tárolásához.
    - hello fiók is lehet standard vagy [prémium](../storage/storage-premium-storage.md).
    - Az erőforrás-kezelőben, vagy a klasszikus módban fiók állíthat be.

3. [Készítse elő a fiók](#prepare-for-automatic-discovery-and-push-installation) hello vCenter-kiszolgáló vagy vSphere rendszert futtató gazdagépeken, ezért a Site Recovery automatikusan felismeri VMware virtuális gépek.

## <a name="prepare-hello-configuration-server"></a>Hello konfigurációs kiszolgáló előkészítése

1. Telepítse a Windows Server 2012 R2 vagy újabb verzióját, a VMware virtuális gép.
2. Ellenőrizze, hogy hello VM rendelkezik toohello URL-címek felsorolt [Előfeltételek](#prerequisites).
3. Telepítés [VMware vSphere PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1).


## <a name="prepare-for-automatic-discovery-and-push-installation"></a>Automatikus felderítés és leküldéses telepítésének előkészítése

- **Készítse elő a fiók automatikus felderítését a**: hello Site Recovery folyamatkiszolgáló automatikusan észleli a virtuális gépek. toodo, a Site Recovery igényeinek hitelesítő adatokat, amelyek egy vCenter-kiszolgáló és az ESXi-gazdagépek vSphere elérhetik.

    1. egy külön fiókot toouse szerepkör létrehozása (szinten hello vCenter, ezekkel [engedélyek](#vmware-account-permissions). Nevezze el, például a **Azure_Site_Recovery**.
    2. Ezután hello vSphere gazdagép/vCenter-kiszolgáló egy felhasználó létrehozása, és rendelje hozzá hello szerepkör toohello felhasználót. Ez a felhasználói fiók a Site Recovery üzembe helyezése során adja meg.

- **Egy fiók toopush hello mobilitásiszolgáltatás előkészítése**: Ha toopush hello mobilitási szolgáltatás tooVMs, kell-e egy hello folyamat server tooaccess hello virtuális gép által használt fiókot. hello fiók hello ügyfélleküldéses telepítés csak szolgál. A tartományi vagy helyi fiók is használhatja:

    - A Windows Ha nem használ egy olyan tartományi fiók kell toodisable távelérési felhasználói vezérlő hello helyi számítógépen. Ez, hello regisztrálni a toodo **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, hello DWORD bejegyzés hozzáadása **LocalAccountTokenFilterPolicy**, 1 értékű.
    - Ha a parancssori felület a Windows hello bejegyzés tooadd szeretne, írja be:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
    - Linux hello fióknak root forráskiszolgálón hello Linux kell lennie.

## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-hello-protection-goal"></a>Hello védelmi cél kiválasztása

Jelölje ki a tooreplicate, és a tooreplicate, ahová.

1. Kattintson a **Recovery Services-tárolók** > tárolóban.
2. Hello erőforrás menüben, kattintson **Site Recovery** > **1. lépés: az infrastruktúra előkészítése** > **védelmi cél**.

    ![Célok megválasztása](./media/site-recovery-vmware-to-azure/choose-goals.png)
3. A **védelmi cél**, jelölje be **tooAzure** > **Igen, amelyen a VMware vSphere Hipervizorra**.

    ![Célok megválasztása](./media/site-recovery-vmware-to-azure/choose-goals2.png)

## <a name="set-up-hello-source-environment"></a>Hello forráskörnyezet beállítása

Hello konfigurációs kiszolgáló, regisztrálja azt hello tárolóban, és virtuális gépek felderítése.

1. Kattintson a **helyreállítási hely** > **1. lépés: infrastruktúra előkészítése** > **forrás**.
2. Ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló**.

    ![A forrás beállítása](./media/site-recovery-vmware-to-azure/set-source1.png)
3. A **kiszolgáló hozzáadása**, ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.
4. Töltse le a hello Site Recovery az egységes telepítő telepítőfájlját.
5. Hello tárolóbeli regisztrációs kulcs letöltése. Ez szükséges az egységes telepítő futtatásakor. hello kulcs a generálásától öt napig esetén érvényes.

   ![A forrás beállítása](./media/site-recovery-vmware-to-azure/set-source2.png)


## <a name="run-site-recovery-unified-setup"></a>Futtassa a Site Recovery egyesített telepítő

Tegye hello következő előtt indítsa el, majd futtassa az egységes telepítő tooinstall hello konfigurációs kiszolgáló, hello folyamatkiszolgáló és fő célkiszolgáló hello.
    - Gyors áttekintő videó beolvasása

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - Hello konfigurációs kiszolgálón VM, győződjön meg arról, hogy hello rendszeróra szinkronizálva van a [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Meg kell felelnie. Ha az előtérben 15 perc vagy mögött található, a telepítés meghiúsulhat.
    - Futtassa a telepítőt a helyi rendszergazdájaként hello virtuális gép konfigurációs kiszolgálón.
    - Győződjön meg arról, hogy a TLS 1.0 engedélyezve van a virtuális gép hello.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> hello konfigurációs kiszolgálón is telepíthető [hello parancssorból](http://aka.ms/installconfigsrv).



### <a name="add-hello-account-for-automatic-discovery"></a>Automatikus felderítés hello fiók hozzáadása

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="connect-toovmware-servers"></a>Csatlakozás tooVMware kiszolgálók

Csatlakozás toovSphere ESXi-gazdagépek és vCenter kiszolgálónak toodiscover VMware virtuális gépek.

- Ha hello vCenter-kiszolgáló vagy vSphere-gazdagép rendszergazdai jogosultságok nélküli fiókkal hello kiszolgálón, a hello fiókot kell ezeket a jogokat engedélyezve:
    - Datacenter, Datastore, mappa, állomás, hálózati, erőforrás, a virtuális gép, vSphere elosztott kapcsoló.
    - hello vCenter-kiszolgálót kell tárolási nézetek engedélyekkel.
- VMware-kiszolgálók hozzáadásakor 15 percbe is telhet, vagy hosszabb ideig őket tooappear hello portálon.
tooallow Azure Site Recovery toodiscover futó virtuális gépek a helyszíni környezetben, kell tooconnect a VMware vCenter-kiszolgáló vagy vSphere ESXi-gazdagépek Site Recovery szolgáltatással.

Válassza ki **+ vCenter** toostart VMware vCenter-kiszolgáló vagy egy VMware vSphere ESXi-állomáson.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

A Site Recovery csatlakoztatja tooVMware kiszolgálókat hello segítségével megadott beállításokat, és felderíti a virtuális gépek.

## <a name="set-up-hello-target"></a>Hello cél beállítása

Hello célkörnyezet beállítása előtt ellenőrizze, hogy van egy [Azure storage-fiók és a hálózat](#set-up-azure)

1. Kattintson a **infrastruktúra előkészítése** > **cél**, és válassza ki a kívánt toouse Azure-előfizetés hello.
2. Adja meg-e a cél telepítési modell a Resource Manager-alapú, vagy a klasszikus.
3. A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.

   ![cél](./media/site-recovery-vmware-to-azure/gs-target.png)
4. Ha még nem hozott létre a storage-fiók vagy a hálózaton, kattintson a **+ tárfiók** vagy **+ hálózat**, egy erőforrás-kezelő fiók vagy a hálózati beágyazott toocreate.

## <a name="set-up-replication-settings"></a>Replikációs beállítások megadása

Gyorsan áttekintheti videó elindítása előtt:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. egy új replikációs házirendet toocreate kattintson **Site Recovery-infrastruktúra** > **replikációs házirendek** > **+ replikációs házirend**.
2. A **replikációs házirend létrehozása**, megadhatja a házirend nevét.
3. A **helyreállítási Időkorlát küszöbértéke**, adja meg a hello RPO korlátot. Ez az érték határozza meg, milyen gyakran adatok helyreállítási pontot hoz létre. Riasztás akkor jön létre, ha a folyamatos replikálásra túllépi ezt a korlátozást.
4. A **helyreállításipont-megőrzést**, adja meg (órában) mennyi ideig áll hello adatmegőrzési időtartam az egyes helyreállítási pontok. A replikált virtuális gépek lehet helyreállítani tooany pont ablakban. Másolatot óra megőrzési gépek esetén támogatott too24 replikált toopremium tárolás és a standard szintű tárolást 72 órát.
5. A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg, milyen gyakran (percben) alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontokat hoz létre. Kattintson a **OK** toocreate hello házirend.

    ![Replikációs szabályzat](./media/site-recovery-vmware-to-azure/gs-replication2.png)
8. Ha egy új házirendet hoz létre, automatikusan rendelkezik hello konfigurációs kiszolgáló társítva. Alapértelmezés szerint a megfelelő házirend automatikusan létrejön a feladat-visszavételre. Például ha hello replikációs házirend nem **rep-házirend** akkor hello feladatátvételi házirendhez lesz **rep-házirend-feladat-visszavétel**. Ez a házirend nem használ, amíg nem indítja el a feladat-visszavétel az Azure-ból.  



## <a name="plan-capacity"></a>Kapacitástervezés

1. Most, hogy az alapszintű infrastruktúra készen áll, gondolja át a kapacitástervezés, és mérje fel, hogy szükséges-e további forrásokat. [További információk](site-recovery-plan-capacity-vmware.md).
2. Amikor elkészült, a kapacitástervezés, válassza ki a **Igen** a **elvégezte a kapacitástervezést?**

   ![Kapacitástervezés](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)


## <a name="prepare-vms-for-replication"></a>Készítse elő a virtuális gépek a replikáció

hello mobilitási szolgáltatást telepíteni kell minden VMware virtuális gépek, amelyet az tooreplicate. Hello mobilitásiszolgáltatás többféle módszerrel is telepíthető:

1. Telepítse a leküldéses telepítés hello folyamat kiszolgálóról. Virtuális gépek toouse tooprepare kell ezt a módszert.
2. A telepítés központi telepítési eszközök, például a System Center Configuration Manager vagy az Azure automation DSC használata.
3.  Manuálisan telepítse.

[További információ](site-recovery-vmware-to-azure-install-mob-svc.md)


## <a name="enable-replication"></a>A replikáció engedélyezése

Előkészületek:

- Az Azure felhasználói fiókot kell toohave bizonyos [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) egy új virtuális gép tooAzure tooenable replikációját.
- Hozzáadásakor, vagy módosítsa a virtuális gépek, is igénybe vehet fel too15 perc vagy már módosítások tootake hatás és a számukra tooappear hello portálon.
- A legutóbb felfedezett hello idő ellenőrizze, hogy a virtuális gépek **konfigurációs kiszolgálók** > **utolsó lépjen kapcsolatba a**.
- virtuális gépek tooadd hello ütemezett felderítési, kiemelési hello konfigurációs kiszolgáló várakozás nélkül (ne kattintson azt), és kattintson a **frissítése**.
- A virtuális gépek kész az ügyfélleküldéses telepítést, ha a hello folyamatkiszolgáló automatikusan telepíti az hello mobilitási szolgáltatás, amikor a replikáció engedélyezése.


### <a name="exclude-disks-from-replication"></a>Lemezek kizárása a replikációból

Alapértelmezés szerint az összes lemez gépen replikálódnak. Lemezek lehet kizárni a replikálásból. Például előfordulhat, hogy nem kívánja tooreplicate lemezek, amelyek ideiglenes vagy frissítette minden alkalommal, amikor olyan gép, adatok, vagy alkalmazás újraindul, (például pagefile.sys vagy SQL Server tempdb).

### <a name="replicate-vms"></a>Virtuális gépek replikálása

Mielőtt nekikezdene, tekintse meg a gyors áttekintő videó

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. Kattintson **2. lépés: Az alkalmazás replikálása** > **Forrás** elemre.
2. A **forrás**, jelölje be hello konfigurációs kiszolgáló.
3. A **számítógép típusú**, jelölje be **virtuális gépek**.
4. A **vCenter vagy vSphere Hipervizorra**, hello vCenter-kiszolgálót, amely hello vSphere-gazdagép felügyeli, vagy válasszon hello állomás.
5. Hello folyamatkiszolgáló kijelölése. Ha még nem hozott létre minden olyan további folyamat kiszolgálóhoz ez lesz hello konfigurációs kiszolgáló. Ezután kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. A **cél**válassza ki a hello előfizetés, hello virtuális gépek a feladatátvételt toocreate hello használni kívánt erőforráscsoportot. Válassza ki a beállítani kívánt toouse (klasszikus vagy erőforrás-kezelés), Azure virtuális gépek a feladatátvételt hello hello telepítési modell.


7. Válassza ki a kívánt toouse adatok replikálásához hello Azure storage-fiók. Ha már beállította fiók toouse nem szeretné, létrehozhat egy újat.

8. SELECT hello Azure hálózati és alhálózati toowhich Azure virtuális gépeken fog csatlakozni, ha a feladatátvételt követően jönnek létre. Válassza ki **beállítás most a kijelölt gépekhez**, tooapply hello beállítás tooall számítógépek választja a védelem. Válassza ki **konfigurálását később** tooselect hello gépenként Azure-hálózatot. Ha egy meglévő hálózati toouse nem szeretné, akkor hozzon létre egyet.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-rep3.png)
9. A **virtuális gépek** > **válassza ki a virtuális gépek**kattintson, és válassza az egyes gépek tooreplicate szeretné. Csak olyan gépeket választhat, amelyeken használható a replikáció funkció. Végül kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication5.png)
10. A **tulajdonságok** > **tulajdonságainak konfigurálása**, jelölje be hello folyamat server tooautomatically által használt fiók hello hello mobilitási szolgáltatás telepítése hello gépen.
11. Alapértelmezés szerint a rendszer replikálja az összes lemez. Kattintson a **összes lemez** , és törölje a nem kívánt tooreplicate lemezzel. Ezután kattintson az **OK** gombra. Virtuális gép további tulajdonságokat később állíthatja be.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication6.png)
11. A **replikációs beállítások** > **replikáció beállításainak konfigurálása**, győződjön meg arról, hogy helyes-e replikációs házirend be van jelölve hello. Ha módosít egy házirendet, a változások lesznek alkalmazott tooreplicating gép és toonew gépek.
12. Engedélyezése **virtuális Gépre kiterjedő konzisztencia** Ha toogather gépeihez egy replikációs csoporthoz, és adjon meg egy nevet hello csoportnak. Ezután kattintson az **OK** gombra. Vegye figyelembe:

    * A replikációs csoportok gépek replikálása együtt, és megosztott összeomlás-konzisztens és alkalmazáskonzisztens helyreállítási pontokat, amikor azok a feladatátvételt.
    * Azt javasoljuk, hogy gyűjtse össze a virtuális gépek és fizikai kiszolgálók együtt, hogy látni lehessen a munkaterhelések. Több virtuális Gépre kiterjedő konzisztencia engedélyezése befolyásolhatja a teljesítményt a munkaterhelés, és csak akkor használja, ha a gépek is működnek hello azonos munkaterhelés, és konzisztenciára van szükség.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication7.png)
13. Kattintson a **engedélyezze a replikálást**. Hello állapotának nyomon követheti **Védelemengedélyezési** feladat **beállítások** > **feladatok** > **Site Recovery-feladatok**. Hello után **Védelemvéglegesítési** feladat futtatása hello gép készen áll a feladatátvételre.

### <a name="view-and-manage-vm-properties"></a>A virtuális gépek tulajdonságainak megtekintése és kezelése

Azt javasoljuk, hogy ellenőrizze hello virtuális gép tulajdonságait, és hajtsa végre a módosításokat kell.

1. Kattintson a **replikált elemek** >, és jelölje be hello gép. Hello **Essentials** panel gépek beállítások és állapotával kapcsolatos adatokat jeleníti meg.
2. A **tulajdonságok**, megtekintheti a replikációs és feladatátvételi adatait hello virtuális gép.
3. A **számítás és hálózat** > **számítási tulajdonságok**, megadhatja a hello Azure virtuális gép nevét és a cél méretét. Hello neve toocomply rendelkező módosítása [Azure-követelményeknek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) Ha kell.
4. Hello célhálózat alhálózati és toohello Azure virtuális Géphez rendelendő IP-cím beállításainak módosítása:

   - Beállíthat hello cél IP-címet.

    - Ha nem ad meg egy címet, a gép a feladatátvételt hello DHCP fogja használni.
    - Ha egy címet, amely nem használható feladatátadásra, feladatátvétel nem fog működni.
    - cél IP-cím a feladatátvételi teszthez, használható, ha hello címkészletében nem áll rendelkezésre a feladatátvételi teszt hálózatában hello hello.

   - hálózati adapterek száma hello hello cél virtuális gép számára megadott hello mérete határozza meg:

     - Ha hálózati adapterek hello forrásgépen hello száma hello ugyanaz, mint a, vagy kevesebb, mint hello hello célként megadott gép méretéhez engedélyezett adapterek számával, akkor hello cél lesz hello ugyanannyi adapter hello forrásaként.
     - Ha hello hello forrás virtuális gép adaptereinek száma meghaladja hello engedélyezett hello célméretet, akkor hello cél maximális használja a rendszer.
     - Például ha a forrásgépen két hálózati adaptert, és hello célgép mérete négy támogatja, akkor hello célgépen két adapter fog működni. Ha hello forrásgépen két adapter, azonban a hello támogatott célméretet csak egy majd hello célgépen csak egy adapter fog működni.     
   - Ha a virtuális gépnek több hálózati adapter hello lesz az összes csatlakoznak toohello ugyanazon a hálózaton.
   - Ha hello a virtuális gépnek a több hálózati adapter, majd hello először egy hello listán látható lesz hello *alapértelmezett* hello Azure virtuális gép hálózati adapteréhez.
4. A **lemezek**, megtekintheti a hello virtuális gép operációs rendszer, és hello adatok lemezek, a rendszer replikálja.

#### <a name="managed-disks"></a>Felügyelt lemezek

A **számítás és hálózat** > **számítási tulajdonságok**, ha azt szeretné, hogy tooattach felügyelt lemezek tooyour gépen a feladatátvevő tooAzure "Az kezelt lemezek" beállítás túl "Yes" a virtuális gép hello állíthatja be. Kezelt lemezeken Azure IaaS virtuális gépek Lemezkezelés leegyszerűsíti hello virtuális gépek lemezei társított hello storage-fiókok kezelése. [További tudnivalók a kezelt lemezek.](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview)

   - Felügyelt lemezek létrehozott és csatolt toohello virtuális gép csak egy feladatátvételi tooAzure. A védelem engedélyezése után a helyszíni gépeket adatokat továbbra is tooreplicate toostorage fiókok.  Felügyelt lemezek csak hello Resource manager üzembe helyezési modellben használatával telepített virtuális gépeket hozható létre.  

   - Ha úgy állítja be "Az kezelt lemezek" túl "Yes", csak rendelkezésre állási készletek hello erőforráscsoportban "Az kezelt lemezek" készlettel túl "Yes" lenne kijelölésnél érhetők el. Ennek az az oka felügyelt lemezzel rendelkező virtuális gépek csak része lehet "Kezelt lemez" tulajdonságkészlet rendelkezésre állási készletek túl "Yes". Győződjön meg arról, hogy hozzon létre rendelkezésre állási csoportok "Kezelt lemez" tulajdonság beállítva a felügyelt leképezési toouse lemezek a feladatátvételi alapján.  Hasonlóképpen ha úgy állítja be "Az kezelt lemezek" túl "nem", csak rendelkezésre állási készletek hello erőforráscsoportban, "Az kezelt lemezek" tulajdonsága túl "nem" lenne kijelölésnél érhetők el. [További információk a felügyelt lemezekről és a rendelkezésre állási csoportokról](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Ha replikációhoz használt hello tárfiók volt titkosítva Storage szolgáltatás titkosítási bármikor időben, a felügyelt lemezek feladatátvétel során sikertelen lesz. Akkor is, vagy állítsa be "Az kezelt lemezek" túl "nem", és ismételje meg a feladatátvételi vagy tiltsa le a védelmet a virtuális gép hello és a védelmét, tooa storage-fiók, amelynek nincs Storage szolgáltatás titkosítási bármikor időben engedélyezve van.
  > [További információk a Storage Service Encryptionről és a felügyelt lemezekről](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása


Miután minden állított be, futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik. Gyorsan áttekintheti videó elindítása előtt
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]


1. toofail keresztül egyetlen számítógépen, a **beállítások** > **replikált elemek**, kattintson a virtuális gép hello > **+ feladatátvételi teszt** ikonra.

    ![A feladatátvételi teszt](./media/site-recovery-vmware-to-azure/TestFailover.png)

1. a helyreállítás alatt toofail tervez, a **beállítások** > **helyreállítási tervek**, kattintson a jobb gombbal hello terv > **feladatátvételi teszt**. a helyreállítási terv toocreate [kövesse ezeket az utasításokat](site-recovery-create-recovery-plans.md).  

1. A **feladatátvételi teszt**, jelölje be hello Azure hálózati toowhich Azure virtuális gépek csatlakoznak feladatátvételt követően.

1. Kattintson a **OK** toobegin hello feladatátvételi. Előrehaladásának parancsával hello VM tooopen tulajdonságát, vagy a hello **feladatátvételi teszt** feladatot a tároló neve > **beállítások** > **feladatok**  >  **Site Recovery-feladatok**.

1. Hello feladatátvétel befejezése után meg kell jelennie toosee hello replika Azure machine hello Azure-portálon jelenik meg > **virtuális gépek**. Győződjön meg arról, hogy a virtuális gép hello hello megfelelő méretét, csatlakoztatva van a megfelelő hálózati toohello, és fut-e.

1. Ha Ön [elvégezte a feladatátvételt követően](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover), meg kell tudni tooconnect toohello Azure virtuális Gépen.

1. Ha elkészült, kattintson a **karbantartása a feladatátvételi teszt** a hello helyreállítási terv. A **megjegyzések**, és a feladatátvételi teszt hello kapcsolatos megfigyelések feljegyzéséhez mentéséhez. Ezzel a lépéssel törli a teszt feladatátvétele során létrehozott hello virtuális gépeket.

[További](site-recovery-test-failover-to-azure.md) vonatkozó feladatátvételi tesztet.


## <a name="vmware-account-permissions"></a>VMware fiókjához hozzárendelt jogosultságoktól

A Site Recovery hozzáférés tooVMware van szüksége, hello folyamat server tooautomatically virtuális gépek felderítése, valamint a feladatátvétel és a feladat-visszavételt a virtuális gépek.

- **Telepítse át**: csak toomigrate VMware virtuális gépek tooAzure, nélkül szeretné legalább egyszer sikertelen vissza őket, ha használható a VMware-fiók egy csak olvasható szerepkör. Ilyen szerepkör feladatátvételi futtatható, de nem lehet leállítani a védett forrásgépek. Ez nem szükséges az áttelepítéshez.
- **Replikálás/Recover**: Ha toodeploy (replikálja, feladatátvétel, feladat-visszavétel) teljes replikáció hello fióknak kell lennie a képes toorun műveletek, például a létrehozásával és lemezek eltávolításával, bekapcsolása a virtuális gépek stb.
- **Automatikus felderítés**: legalább egy csak olvasható fiókot kell megadni.


**Tevékenység** | **Szükséges fiók/szerepkör** | **Engedélyek** | **Részletek**
--- | --- | --- | ---
**A folyamatkiszolgáló automatikusan felderíti a VMware virtuális gépek** | Legalább egy írásvédett felhasználó van szüksége | Adatközpont objektum –> propagálása tooChild objektum szerepkör = csak olvasható | Felhasználó datacenter szinten hozzárendelt, és hozzáférést tooall hello objektumok hello adatközpontban.<br/><br/> toorestrict hozzáférés hozzárendelése hello **nem lehet hozzáférni** hello szerepkört **toochild propagálása** objektum toohello gyermekobjektum (vSphere-gazdagép, datastores, virtuális gépek és hálózatok).
**Feladatátvétel** | Legalább egy írásvédett felhasználó van szüksége | Adatközpont objektum –> propagálása tooChild objektum szerepkör = csak olvasható | Felhasználó datacenter szinten hozzárendelt, és hozzáférést tooall hello objektumok hello adatközpontban.<br/><br/> toorestrict hozzáférés hozzárendelése hello **nem lehet hozzáférni** hello szerepkört **toochild propagálása** toohello gyermekobjektum (vSphere-gazdagép, datastores, virtuális gépek és hálózatok) objektum.<br/><br/> Akkor hasznos, ha az áttelepítési célokból, de nem a teljes replikáció, feladatátvétel, feladat-visszavétel.
**A feladatátvételi és a feladat-visszavétel** | Javasoljuk, hogy (Azure_Site_Recovery) szerepkör létrehozása hello szükséges engedélyekkel, és hozzárendelheti a hello szerepkör tooa VMware felhasználó vagy csoport | Adatközpont objektum –> propagálása tooChild objektum szerepkör = Azure_Site_Recovery<br/><br/> Datastore foglaljon le terület ->, keresse meg az adattároló, alacsony szintű fájlműveletek, távolítsa el a fájlt, frissítse a virtuális géphez tartozó fájlokat<br/><br/> Hálózat -> hálózat hozzárendelése<br/><br/> Erőforrás -> rendelje hozzá a virtuális gép tooresource készlet, energiaforrással rendelkező ki a virtuális gép áttelepítése, energiaforrással rendelkező, a virtuális gép áttelepítése<br/><br/> Feladatok -> hozzon létre feladat, a frissítési feladat<br/><br/> Virtuális gép konfigurációja -><br/><br/> Virtuálisgép -> kommunikáció -> válasz kérdést, az eszköz kapcsolat, a CD media konfigurálásához, a konfigurálásához a hajlékonylemezes adathordozót, kapcsolja ki, a bekapcsolás, VMware-eszközök telepítése<br/><br/> Virtuálisgép -> leltár -> létrehozási, regisztrálása, unregister<br/><br/> Virtuálisgép -> kiépítés engedélyezése virtuális gép letöltési ->, hogy a virtuálisgép-fájlok feltöltése<br/><br/> Virtuális gép pillanatkép ->-Remove pillanatképek > | Felhasználó datacenter szinten hozzárendelt, és hozzáférést tooall hello objektumok hello adatközpontban.<br/><br/> toorestrict hozzáférés hozzárendelése hello **nem lehet hozzáférni** hello szerepkört **toochild propagálása** objektum toohello gyermekobjektum (vSphere-gazdagép, datastores, virtuális gépek és hálózatok).


## <a name="next-steps"></a>Következő lépések

Miután beolvasni replikációs, és fut, nem tervezett kimaradás esetén a rendszer átadja a tooAzure, és Azure virtuális gépek replikálása hello adatokból jönnek létre. Amíg hátsó tooyour elsődleges hely sikertelen, a rendszer visszaadja toonormal műveletek munkaterheléseket és alkalmazásokat az Azure majd hozzáférni.

- [További](site-recovery-failover.md) kapcsolatos különféle a feladatátvételeket, és hogyan toorun őket.
- Ha a gépek replikálásához helyett és visszavétele, amelybe migrálna [további](site-recovery-migrate-to-azure.md#migrate-on-premises-vms-and-physical-servers).
- [További információ a feladat-visszavétel](site-recovery-failback-azure-to-vmware.md), toofail vissza és replikálása Azure virtuális gépek biztonsági toohello helyszíni elsődleges hely.

## <a name="third-party-software-notices-and-information"></a>Harmadik felek szoftvereivel, megjegyzések és információk
Ne Localize vagy lefordítása

hello szoftver és a belső vezérlőprogram futó hello Microsoft termék vagy szolgáltatás alapul, vagy a szerződés magában foglalja a hello anyag projektek lenti (együttesen "harmadik féltől származó kód").  A Microsoft hello hello harmadik féltől származó kód nem eredeti szerzője.  hello eredeti szerzői és a licenc, amely alatt a Microsoft az ilyen harmadik féltől származó kód kapott alábbi van beállítva.

hello leírtak A harmadik féltől származó kód hello projektek-összetevőt az alább felsorolt kapcsolódik. Ilyen licencek és információkat csak tájékoztatási célokat szolgálnak.  A harmadik féltől származó kód folyamatban van a Microsoft szoftverlicenc-feltételek hello Microsoft termék vagy szolgáltatás a Microsoft által relicensed tooyou.  

b. hello információk kapcsolatos harmadik féltől származó kód összetevők végrehajtott elérhető tooyou Microsoft hello eredeti licencelési feltételei szerint.

hello teljes fájl is található a hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). A Microsoft kifejezetten át nem adott, az összes jogot fenntart által Ennek következménye, hogy belül vagy más módon.
