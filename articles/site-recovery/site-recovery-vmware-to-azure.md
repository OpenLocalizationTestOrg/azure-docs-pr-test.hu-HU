---
title: "VMware virtuális gépek replikálása az Azure-bA |} Microsoft Docs"
description: "Szükség van az Azure storage VMware virtuális gépeken futó számítási feladatok replikálása ismertetett lépéseket foglalja"
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
ms.openlocfilehash: 8a312f3c1a65b2a9bb91abbfd8b18fb0ec0279b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-vmware-virtual-machines-to-azure-with-site-recovery"></a>VMware virtuális gépek replikálása az Azure Site Recovery szolgáltatással

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmware-to-azure.md)
> * [Klasszikus Azure portál](site-recovery-vmware-to-azure-classic.md)


Ez a cikk ismerteti a helyszíni VMware virtuális gépek replikálása az Azure-ba, használja a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás az Azure portálon.

Ha VMware virtuális gépek áttelepítése az Azure-ba (csak feladatátvételi) szeretne, olvassa el [Ez a cikk](site-recovery-migrate-to-azure.md) további.

Ebben a cikkben, vagy az alsó megjegyzések és kérdéseket küldje a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="deployment-steps"></a>A központi telepítés lépései

Ez mit kell tennie:

1. Előfeltételek és korlátozások ellenőrzése.
2. Azure-hálózat és tárfiókok beállítása.
3. Készítse elő a a helyi számítógépen, mint a konfigurációs kiszolgálót telepíteni szeretné.
4. Készítse elő a VMware-fiókok a virtuális gépek automatikus felderítése használható, és opcionálisan leküldéses telepíteni a mobilitási szolgáltatást.
4. Recovery Services-tároló létrehozása. A tároló konfigurációs beállításokat tartalmaz, és koordinálja a replikációt.
5. Adja meg a forrás, a cél és a replikációs beállításokat.
6. A mobilitási szolgáltatást a replikálni kívánt virtuális gépeket telepíthet.
7. Engedélyezze a replikálást a virtuális gépek számára.
7. Egy feladatátvételi teszt futtatásával ellenőrizze, hogy minden a vártnak megfelelően működik-e.

## <a name="prerequisites"></a>Előfeltételek

**Támogatási követelmények** | **Részletek**
--- | ---
**Azure** | További tudnivalók [Azure-követelmények](site-recovery-prereq.md#azure-requirements)
**Helyszíni konfigurációs kiszolgáló** | Windows Server 2012 R2 rendszerű VMware virtuális gép van szüksége, vagy később. Ez a kiszolgáló beállítása a Site Recovery üzembe helyezése során.<br/><br/> Alapértelmezés szerint a folyamatkiszolgáló és fő célkiszolgáló is települnek a virtuális Gépet. Növelheti, ha szüksége lehet különálló folyamatkiszolgálót, és nem rendelkezik, ugyanazok a követelmények vonatkoznak, mint a konfigurációs kiszolgálót.<br/><br/> További információ ezen összetevők [Itt](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)
**Helyszíni VMware-kiszolgálók** | Egy vagy több VMware vSphere, futtató kiszolgálók 6.0, 5.5, 5.1 legújabb frissítéseit. Kiszolgálók ugyanazon a hálózaton, mint a konfigurációs kiszolgáló (vagy különálló folyamatkiszolgálót) kell elhelyezni.<br/><br/> Azt javasoljuk, hogy a vCenter-kiszolgálót a legújabb frissítésekkel rendelkező 6.0 vagy 5.5 rendszerű gazdagépek felügyeletéhez. Csak azok a szolgáltatások által biztosított 5.5 6.0-s verzió telepítésekor támogatottak.
**A helyszíni virtuális gépek** | A replikálni kívánt virtuális gépeknek [támogatott operációs rendszert](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) kell futtatniuk, valamint meg kell felelniük az [Azure-előfeltételeknek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). Virtuális gép VMware-eszközök futó kell rendelkeznie.
**URL-címek** | A konfigurációs kiszolgáló URL-hozzáférésre van szüksége:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Ha IP-címeken alapuló tűzfalszabályokat használ, ellenőrizze, hogy engedélyezik-e az Azure-ral való kommunikációt.<br/></br> Engedélyezze az [Azure-adatközpont IP-tartományait](https://www.microsoft.com/download/confirmation.aspx?id=41653) és a HTTPS-portot (443).<br/></br> Engedélyezze az előfizetéshez tartozó Azure-régió, illetve az USA nyugati régiójának IP-tartományait (a hozzáférés-vezérléshez és az identitáskezeléshez szükséges).<br/><br/> Az URL-címet a MySQL Letöltés: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Mobilitási szolgáltatás** | Minden replikált virtuális gép telepítve.




## <a name="limitations"></a>Korlátozások

**A korlátozás** | **Részletek**
--- | ---
**Azure** | Tárolási és hálózati fiókok és a tárolónak ugyanabban a régióban kell lennie.<br/><br/> A prémium szintű storage-fiókok használatakor is szabványos store-fiók szükséges replikálási naplók tárolásához<br/><br/> Közép-és Dél-Indiában premium-fiókok nem replikálja.
**Helyszíni konfigurációs kiszolgáló** | VMware virtuális hálózatiadapter-típus VMXNET3 kell lennie. Ha nem, [a frissítés telepítése](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)<br/><br/> a vSphere PowerCLI 6.0 kell telepíteni.<br/><br> A gép nem lehet tartományvezérlő. A gépnek rendelkeznie kell egy statikus IP-címet.<br/><br/> Az állomásnév 15 karakter lehet vagy kevesebb, és az operációs rendszer angol nyelven kell lennie.
**VMware** | VCenter 6.0 csak 5.5 szolgáltatások támogatottak. A Site Recovery nem támogatja az új vCenter és vSphere 6.0 szolgáltatások például a kereszt-vCenter vMotion, a virtuális kötetek és a tárolási DRS.
**Virtuális gépek** | Győződjön meg arról [Azure virtuális gép korlátozásai](site-recovery-prereq.md#azure-requirements)<br/><br/> Titkosított lemezzel rendelkező virtuális gépeket, vagy az UEFI/EFI-rendszerbetöltés virtuális gépeken nem replikálja.<br/><br> A megosztott lemez nem használhatók. Ha a forrás virtuális gép hálózati adapterek összevonása, akkor alakítja át egyetlen hálózati adapter a feladatátvételt követően.<br/><br/> Ha virtuális gépek iSCSI-lemez, a Site Recovery átalakítja a VHD-fájlt a feladatátvételt követően. Ha az iSCSI-cél által az Azure virtuális gép elérhető, akkor kapcsolódik, és látja, és a virtuális Merevlemezt. Ha ez történik, válassza le az iSCSI-tároló.<br/><br/> Ahhoz, hogy több virtuális Gépre kiterjedő konzisztencia, amely lehetővé teszi a virtuális gépeket futtathatnak ugyanaz az alkalmazás konzisztens adatponthoz együtt helyre kell állítani, nyissa meg a virtuális gép 20004 portjához.<br/><br/> A Windows a C meghajtó telepítve kell lennie. Az operációs rendszer lemezének alapvető, és nem dinamikus kell lennie. Az adatok lemez lehet dinamikus.<br/><br/> Linux/Etc/Hosts fájlt a virtuális gépek bejegyzéseit, amelyek társított összes hálózati adapter IP-címek hozzárendelését a helyi állomás nevét kell tartalmaznia. Az állomásnév, csatlakoztatási pontokat, eszköz, elérési utak, és fájlnevek (/ etc; / usr) kell lennie csak angol nyelven.<br/><br/> Adott típusú [Linux tárolási](site-recovery-support-matrix-to-azure.md#support-for-storage) támogatottak.<br/><br/>Hozzon létre, vagy állítsa be **disk.enableUUID=true** a virtuális gép beállításai. Ez biztosít egy egységes UUID a vmdk-fájl, hogy helyesen csatlakoztatja, és biztosítja, hogy csak a változási különbözeteket vannak visszakerül a helyszíni feladat-visszavétel, anélkül, hogy a teljes replikáció során.

## <a name="set-up-azure"></a>Azure beállítása

1. [Állítsa be az Azure-hálózatot](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).
    - Azure virtuális gépek kerülnek ehhez a hálózathoz, ha a feladatátvételt követően jönnek létre.
    - Beállíthatja a hálózat [erőforrás-kezelő](../resource-manager-deployment-model.md), vagy a klasszikus mód.

2. Állítson be egy [Azure storage-fiók](../storage/storage-create-storage-account.md#create-a-storage-account) a replikált adatok tárolásához.
    - A fiók lehet normál vagy [prémium](../storage/storage-premium-storage.md).
    - Az erőforrás-kezelőben, vagy a klasszikus módban fiók állíthat be.

3. [Készítse elő a fiók](#prepare-for-automatic-discovery-and-push-installation) a vCenter-kiszolgáló vagy vSphere-gazdagép, így a Site Recovery automatikusan felismeri VMware virtuális gépek.

## <a name="prepare-the-configuration-server"></a>A konfigurációs kiszolgáló előkészítése

1. Telepítse a Windows Server 2012 R2 vagy újabb verzióját, a VMware virtuális gép.
2. Ellenőrizze, hogy a virtuális gép hozzáfér a felsorolt URL-címek [Előfeltételek](#prerequisites).
3. Telepítés [VMware vSphere PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1).


## <a name="prepare-for-automatic-discovery-and-push-installation"></a>Automatikus felderítés és leküldéses telepítésének előkészítése

- **Készítse elő a fiók automatikus felderítését a**: A Site Recovery folyamatkiszolgáló automatikusan észleli a virtuális gépek. Ehhez az szükséges, a Site Recovery szükséges hitelesítő adatokat, amelyek egy vCenter-kiszolgáló és az ESXi-gazdagépek vSphere elérhetik.

    1. Egy külön fiókot használja, hozzon létre egy szerepkört (a vCenter szintjén ezekkel [engedélyek](#vmware-account-permissions). Nevezze el, például a **Azure_Site_Recovery**.
    2. Ezután hozzon létre egy felhasználót a vSphere-gazdagép vagy vCenter Server, és a szerepkör hozzárendeléséhez a felhasználóhoz. Ez a felhasználói fiók a Site Recovery üzembe helyezése során adja meg.

- **Készítse elő a mobilitási szolgáltatás leküldéses fiók**: Ha a mobilitási szolgáltatás leküldéses a virtuális gépekhez, kell-e egy fiókot, amelyet a virtuális gép eléréséhez használható a folyamatkiszolgáló. A fiók csak szolgál a leküldéses telepítés. A tartományi vagy helyi fiók is használhatja:

    - A Windows Ha nem használ egy olyan tartományi fiók szeretné tiltsa le a távoli felhasználói hozzáférés-vezérlés a helyi számítógépen. Ehhez a nyilvántartásában **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, adja hozzá a DWORD bejegyzést **LocalAccountTokenFilterPolicy**, 1 értékű.
    - Ha azt szeretné, a beállításjegyzék-bejegyzés hozzáadása a Windows a a CLI-t, írja be:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
    - A Linux a fióknak kell lennie a forráskiszolgálón Linux legfelső szintű.

## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-the-protection-goal"></a>Védelmi cél kiválasztása

Válassza ki, hogy mit szeretne replikálni, és hová.

1. Kattintson a **Recovery Services-tárolók** > tárolóban.
2. Az erőforrás menüjében kattintson **Site Recovery** > **1. lépés: az infrastruktúra előkészítése** > **védelmi cél**.

    ![Célok megválasztása](./media/site-recovery-vmware-to-azure/choose-goals.png)
3. A **védelmi cél**, jelölje be **az Azure-bA** > **Igen, amelyen a VMware vSphere Hipervizorra**.

    ![Célok megválasztása](./media/site-recovery-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a>A forráskörnyezet beállítása

Állítsa be a konfigurációs kiszolgáló, és regisztrálja őket a tárolóban lévő virtuális gépek felderítése.

1. Kattintson a **helyreállítási hely** > **1. lépés: infrastruktúra előkészítése** > **forrás**.
2. Ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló**.

    ![A forrás beállítása](./media/site-recovery-vmware-to-azure/set-source1.png)
3. A **kiszolgáló hozzáadása**, ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.
4. Töltse le a Site Recovery az egységes telepítő telepítőfájlját.
5. Töltse le a tárolóregisztrációs kulcsot. Ez szükséges az egységes telepítő futtatásakor. A kulcs a generálásától számított öt napig érvényes.

   ![A forrás beállítása](./media/site-recovery-vmware-to-azure/set-source2.png)


## <a name="run-site-recovery-unified-setup"></a>Futtassa a Site Recovery egyesített telepítő

Tegye a következőket előtt indítsa el, majd az egységes telepítőjének futtatásával telepítse a konfigurációs kiszolgáló, a folyamatkiszolgáló és a fő célkiszolgáló.
    - Gyors áttekintő videó beolvasása

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - A konfigurációs kiszolgálón VM, győződjön meg arról, hogy a rendszer órája szinkronizálva van a [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Meg kell felelnie. Ha az előtérben 15 perc vagy mögött található, a telepítés meghiúsulhat.
    - Futtassa a telepítőt a helyi rendszergazda, a virtuális gép konfigurációs kiszolgálón.
    - Győződjön meg arról, hogy a TLS 1.0 engedélyezve van a virtuális Gépen.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> A konfigurációs kiszolgáló is telepíthető [a parancssorból](http://aka.ms/installconfigsrv).



### <a name="add-the-account-for-automatic-discovery"></a>Adja hozzá a fiókot a automatikus felderítése

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="connect-to-vmware-servers"></a>VMware-kiszolgálókkal

VMware virtuális gépek felderítése vSphere ESXi-gazdagépek és vCenter-kiszolgáló kapcsolódni.

- Ha a vCenter-kiszolgáló vagy vSphere-gazdagép rendszergazdai jogosultságok nélküli fiókkal ad hozzá a kiszolgálón, a fiók ezeket a jogokat engedélyezett van szüksége:
    - Datacenter, Datastore, mappa, állomás, hálózati, erőforrás, a virtuális gép, vSphere elosztott kapcsoló.
    - A vCenter-kiszolgálót kell tárolási nézetek engedélyekkel.
- VMware-kiszolgálók hozzáadásakor 15 percbe is telhet, vagy hosszabb, hogy megjelenjenek a portálon.
Ahhoz, hogy az Azure Site Recovery számára a helyszíni környezetben futó virtuális gépek felderítése, meg kell kapcsolni a VMware vCenter-kiszolgáló vagy vSphere ESXi-gazdagépek a Site Recovery.

Válassza ki **+ vCenter** elindítani a VMware vCenter-kiszolgáló vagy egy VMware vSphere ESXi-állomáson.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

A Site Recovery VMware-kiszolgálók, a megadott beállítások csatlakozik, és felderíti a virtuális gépek.

## <a name="set-up-the-target"></a>A cél beállítása

A célkörnyezet beállítása előtt ellenőrizze, hogy van egy [Azure storage-fiók és a hálózat](#set-up-azure)

1. Kattintson az **Infrastruktúra előkészítése** > **Cél** elemre, majd válassza ki a használni kívánt Azure-előfizetést.
2. Adja meg-e a cél telepítési modell a Resource Manager-alapú, vagy a klasszikus.
3. A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.

   ![cél](./media/site-recovery-vmware-to-azure/gs-target.png)
4. Ha még nem hozott létre a storage-fiók vagy a hálózaton, kattintson a **+ tárfiók** vagy **+ hálózat**, az erőforrás-kezelő fiók létrehozása vagy szövegközi hálózati.

## <a name="set-up-replication-settings"></a>Replikációs beállítások megadása

Gyorsan áttekintheti videó elindítása előtt:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. Hozzon létre egy új replikációs házirendet, kattintson a **Site Recovery-infrastruktúra** > **replikációs házirendek** > **+ replikációs házirend**.
2. A **replikációs házirend létrehozása**, megadhatja a házirend nevét.
3. Az **RPO küszöbértéke** beállításnál adja meg az RPO-korlátot. Ez az érték határozza meg, milyen gyakran adatok helyreállítási pontot hoz létre. Riasztás akkor jön létre, ha a folyamatos replikálásra túllépi ezt a korlátozást.
4. A **helyreállításipont-megőrzést**, adja meg (órában) mennyi az adatmegőrzési időtartam legyen az egyes helyreállítási pontok. A replikált virtuális gépek bármelyik pontra ablakban állíthatók helyre. Akár 24 óra megőrzési replikálva prémium szintű Storage, és standard szintű tárolást 72 órát gépek esetén támogatott.
5. A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg, milyen gyakran (percben) alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontokat hoz létre. Kattintson a **OK** a szabályzat létrehozásához.

    ![Replikációs szabályzat](./media/site-recovery-vmware-to-azure/gs-replication2.png)
8. Új házirend létrehozásakor, a konfigurációs kiszolgáló automatikusan ki van társítva. Alapértelmezés szerint a megfelelő házirend automatikusan létrejön a feladat-visszavételre. Például, ha a replikációs házirend **rep-házirend** akkor rendszer a feladat-visszavétel házirend **rep-házirend-feladat-visszavétel**. Ez a házirend nem használ, amíg nem indítja el a feladat-visszavétel az Azure-ból.  



## <a name="plan-capacity"></a>Kapacitástervezés

1. Most, hogy az alapszintű infrastruktúra készen áll, gondolja át a kapacitástervezés, és mérje fel, hogy szükséges-e további forrásokat. [További információk](site-recovery-plan-capacity-vmware.md).
2. Amikor elkészült, a kapacitástervezés, válassza ki a **Igen** a **elvégezte a kapacitástervezést?**

   ![Kapacitástervezés](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)


## <a name="prepare-vms-for-replication"></a>Készítse elő a virtuális gépek a replikáció

VMware virtuális gépeken minden replikálni kívánt kell telepíteni a mobilitási szolgáltatást. A mobilitási szolgáltatás számos módon telepíthető:

1. Telepítse a leküldéses telepítés adatok a folyamatkiszolgálótól. Elő kell készíteni a virtuális gépek, a módszer használatához.
2. A telepítés központi telepítési eszközök, például a System Center Configuration Manager vagy az Azure automation DSC használata.
3.  Manuálisan telepítse.

[További információ](site-recovery-vmware-to-azure-install-mob-svc.md)


## <a name="enable-replication"></a>A replikáció engedélyezése

Előkészületek:

- Egyes rendelkeznie kell Azure felhasználói fiókja [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) engedélyezni szeretné egy új virtuális gép az Azure-bA replikálását.
- Ha fel, vagy módosítsa a virtuális gépek, is eltarthat, és 15 perc vagy már módosítások érvénybe lépéséhez, és hogy megjelenjenek a portálon.
- A legutóbb felfedezett ellenőrizze, hogy a virtuális gépek **konfigurációs kiszolgálók** > **utolsó lépjen kapcsolatba a**.
- Az ütemezett felderítési várakozás nélkül adja hozzá a virtuális gépeket, jelölje ki a konfigurációs kiszolgálót (ne kattintson azt), és kattintson **frissítése**.
- A virtuális gépek kész az ügyfélleküldéses telepítést, ha a folyamatkiszolgáló automatikusan telepíti a mobilitási szolgáltatást, amikor a replikáció engedélyezése.


### <a name="exclude-disks-from-replication"></a>Lemezek kizárása a replikációból

Alapértelmezés szerint az összes lemez gépen replikálódnak. Lemezek lehet kizárni a replikálásból. Például előfordulhat, hogy nem kívánt lemezek, amelyek ideiglenes vagy adatok, amelyek frissítette minden alkalommal, amikor olyan gép replikálása, vagy alkalmazás újraindul, (például pagefile.sys vagy SQL Server tempdb).

### <a name="replicate-vms"></a>Virtuális gépek replikálása

Mielőtt nekikezdene, tekintse meg a gyors áttekintő videó

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. Kattintson **2. lépés: Az alkalmazás replikálása** > **Forrás** elemre.
2. A **forrás**, válassza ki a konfigurációs kiszolgálót.
3. A **számítógép típusú**, jelölje be **virtuális gépek**.
4. A **vCenter vagy vSphere Hipervizorra**jelölje be a vCenter-kiszolgáló, amely kezeli a vSphere-gazdagép, vagy jelölje ki a gazdagépet.
5. Válassza ki a folyamatkiszolgáló. Ha még nem hozott létre minden olyan további folyamat kiszolgálóhoz ez lesz a konfigurációs kiszolgáló. Ezután kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. A **cél**, válassza ki az előfizetés és az erőforráscsoport, amelyben a nem sikerült létrehozni a virtuális gépek keresztül szeretné. A (klasszikus vagy erőforrás-kezelés), az Azure-ban használni kívánt telepítési modell kiválasztása a feladatait átadó virtuális gépeket.


7. Válassza ki az adatok replikálásához használni kívánt Azure-tárfiókot. Ha nem szeretné, már létrehozott fiók használatára, létrehozhat egy újat.

8. Válassza ki azt az Azure-hálózatot, valamint alhálózatot, amelyhez a feladatátvételt követően felálló Azure virtuális gépek csatlakozni fognak. Ha a megadott hálózati beállításokat az összes védelemre kijelölt gépre szeretné alkalmazni, válassza a **Beállítás most a kijelölt gépekhez** lehetőséget. Ha az egyes gépeknél külön-külön szeretné beállítani az Azure-hálózatot, kattintson a **Beállítás később** elemre. Ha nem szeretné használni a meglévő hálózat, létrehozhat egyet.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-rep3.png)
9. A **Virtuális gépek** > **Virtuális gépek kijelölése** menüben kattintással jelölje ki a replikálni kívánt virtuális gépeket. Csak olyan gépeket választhat, amelyeken használható a replikáció funkció. Végül kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication5.png)
10. A **tulajdonságok** > **tulajdonságainak konfigurálása**, válassza ki a fiókot, amelyek használják a folyamatkiszolgáló automatikusan telepíteni a mobilitási szolgáltatás a számítógépen.
11. Alapértelmezés szerint a rendszer replikálja az összes lemez. Kattintson a **összes lemez** , és törölje a lemezek nem szeretne replikálni. Ezután kattintson az **OK** gombra. Virtuális gép további tulajdonságokat később állíthatja be.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication6.png)
11. A **replikációs beállítások** > **replikáció beállításainak konfigurálása**, győződjön meg arról, hogy a megfelelő replikációs házirend van-e kiválasztva. Ha módosít egy házirendet, módosításai gép replikálódik, és új gépre lépnek érvénybe.
12. Engedélyezése **virtuális Gépre kiterjedő konzisztencia** Ha azt szeretné, hogy gyűjtse össze a gépet egy replikációs csoporthoz, és adja meg a csoport nevét. Ezután kattintson az **OK** gombra. Vegye figyelembe:

    * A replikációs csoportok gépek replikálása együtt, és megosztott összeomlás-konzisztens és alkalmazáskonzisztens helyreállítási pontokat, amikor azok a feladatátvételt.
    * Azt javasoljuk, hogy gyűjtse össze a virtuális gépek és fizikai kiszolgálók együtt, hogy látni lehessen a munkaterhelések. Lehetővé teszi több virtuális Gépre kiterjedő konzisztencia befolyásolhatja a teljesítményt a munkaterhelés, és csak akkor használja, ha a gép ugyanaz az alkalmazás fut, és konzisztenciára van szükség.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication7.png)
13. Kattintson a **engedélyezze a replikálást**. Előrehaladásának nyomon követheti a **Védelemengedélyezési** feladat **beállítások** > **feladatok** > **Site Recovery-feladatok**. A **Védelem véglegesítése** feladat befejeződését követően a gép készen áll a feladatátvételre.

### <a name="view-and-manage-vm-properties"></a>A virtuális gépek tulajdonságainak megtekintése és kezelése

Azt javasoljuk, hogy ellenőrizze a virtuális gép tulajdonságait, és hajtsa végre a módosításokat kell.

1. Kattintson a **replikált elemek** >, és jelölje ki a gépet. A **Essentials** panel gépek beállítások és állapotával kapcsolatos adatokat jeleníti meg.
2. A **Tulajdonságok** résznél tekintheti meg a virtuális gép replikációs és feladatátvételi adatait.
3. A **Számítás és hálózat** > **Számítási tulajdonságok** résznél adhatja meg az Azure virtuális gép nevét és a cél méretét. Ha szükséges, írja át úgy a nevet, hogy az megfeleljen az [Azure-követelményeknek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
4. A célhálózat alhálózati és az Azure virtuális géphez rendelendő IP-cím beállításainak módosítása:

   - A cél IP-címe beállítható.

    - Ha nem ad meg címet, a gép, amelynek a feladatait átadja, a DHCP-t fogja használni.
    - Ha egy címet, amely nem használható feladatátadásra, feladatátvétel nem fog működni.
    - Az azonos cél IP-címet a feladatátvételi teszthez, használható, ha a cím érhető el a feladatátvételi teszt hálózatában.

   - A hálózati adapterek számát adja meg, ha a cél virtuális gép méretét határozza meg:

     - Ha a forrásgépen hálózati adapterek száma azonos, vagy annál kisebb, mint a célgép mérete, akkor a cél engedélyezett adapterek számát a ugyanannyi adapter lesz a forrásként.
     - Ha a forrás virtuális gép adaptereinek száma meghaladja a cél méretéhez engedélyezett akkor a cél maximális használja a rendszer.
     - Ha például a forrásgépen két hálózati adapter működik, és a célgép mérete négy adapter használatát teszi lehetővé, a célgépen két adapter fog működni. Ha azonban a forrásgépen két adapter működik, de a cél csupán egy adaptert támogat, akkor a célgépen is csak egy adapter fog működni.     
   - Ha a virtuális gépnek több hálózati adapter lesz az összes csatlakoznak az ugyanazon a hálózaton.
   - Ha a virtuális gépnek több hálózati adaptert, majd az elsőt a listán látható lesz a *alapértelmezett* az Azure virtuális gép hálózati adapteréhez.
4. A **lemezek**, láthatja, hogy a virtuális gép operációs rendszer, és az adatok lemezek, a rendszer replikálja.

#### <a name="managed-disks"></a>Felügyelt lemezek

A **számítás és hálózat** > **számítási tulajdonságok**, "Az kezelt lemezek" beállítást az "Igen" a virtuális gép is beállíthat, ha a kezelt lemezek csatolja a gép a feladatátvételt az Azure-bA. A felügyelt lemezek leegyszerűsítik az Azure IaaS virtuális gépek lemezeinek kezelését a virtuális gép lemezeihez társított Storage-fiókok kezelésével. [További tudnivalók a kezelt lemezek.](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview)

   - Felügyelt lemezek csak az Azure-ba történő feladatátvételkor jönnek létre és kapcsolódnak a virtuális géphez. Védelem engedélyezésekor a helyszíni gépek adatainak a Storage-fiókokba replikálása folytatódik.  Felügyelt lemezeket csak a Resource Manager-alapú üzemi modellel üzembe helyezett virtuális gépeken lehet létrehozni.  

   - Amikor a „Felügyelt lemezek használata” területen az „Igen” lehetőséget választja, az erőforráscsoportnak csak azok a rendelkezésre állási csoportjai lesznek elérhetőek a kiválasztásra, amelyeknél a „Felügyelt lemezek használata” „Igen” értékűre van állítva. Ennek az az oka, hogy a felügyelt lemezekkel rendelkező virtuális gépek csak olyan rendelkezésre állási csoportoknak lehetnek részei, amelyek esetén a „Felügyelt lemezek használata” tulajdonság „Igen” értékűre van állítva. Győződjön meg róla, hogy a rendelkezésre állási csoportokat olyan „Felügyelt lemezek használata” tulajdonságbeállítással hozza létre, ahogyan a feladatátvételkor a felügyelt lemezeket használni kívánja.  Hasonlóképpen, amikor a „Felügyelt lemezek használata” területen a „Nem” lehetőséget választja, az erőforráscsoportnak csak azok a rendelkezésre állási csoportjai lesznek elérhetőek kiválasztásra, amelyeknél a „Felügyelt lemezek használata” tulajdonság „Nem” értékűre van állítva. [További információk a felügyelt lemezekről és a rendelkezésre állási csoportokról](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Ha a replikálásra használt Storage-fiókot bármikor a Storage Service Encryptionnel titkosították, a felügyelt lemezek létrehozása sikertelen lesz a feladatátvételkor. Vagy állítsa a „Felügyelt lemezek használatát” „Nem” értékűre, és próbálja meg újra a feladatátvételt, vagy tiltsa le a virtuális gép védelmét, és védje azt egy olyan Storage-fiókkal, amely esetében soha nem volt engedélyezve a Storage Service Encryption.
  > [További információk a Storage Service Encryptionről és a felügyelt lemezekről](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása


Miután minden állított be, győződjön meg arról, hogy a feladatátvételi teszt futtatása minden a várt módon működik. Gyorsan áttekintheti videó elindítása előtt
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]


1. Az egyetlen számítógépen, feladatátvételt **beállítások** > **replikált elemek**, kattintson a virtuális gép > **+ feladatátvételi teszt** ikonra.

    ![A feladatátvételi teszt](./media/site-recovery-vmware-to-azure/TestFailover.png)

1. Helyreállítási terv feladatátadásához a **Beállítások** > **Helyreállítási tervek** menüben kattintson jobb gombbal a kívánt tervre, majd válassza a **Feladatátvételi teszt** lehetőséget. Helyreállítási terv létrehozásához [kövesse ezeket az utasításokat](site-recovery-create-recovery-plans.md).  

1. A **feladatátvételi teszt**, válassza ki a feladatátvételt követően csatlakoznak az Azure virtuális gépek mely Azure-beli hálózatot.

1. A feladatátvételi művelet elindításához kattintson az **OK** gombra. Előrehaladásának a virtuális gép tulajdonságainak megnyitásához, vagy a gombra kattintva a **feladatátvételi teszt** feladatot a tároló neve > **beállítások** > **feladatok**  >  **Site Recovery-feladatok**.

1. A feladatátvétel befejezését követően a replika Azure-gépnek meg kell jelennie az Azure Portal > **Virtuális gépek** részében. Ellenőrizze, hogy a virtuális gép mérete megfelelő-e, hogy a gép a megfelelő hálózathoz csatlakozik-e, illetve, hogy fut-e.

1. Ha [elvégezte a feladatátvételt követő csatlakozáshoz szükséges előkészületeket](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover), most tudnia kell csatlakozni az Azure virtuális géphez.

1. Miután elkészült, a helyreállítási terven kattintson a **Feladatátvételi teszt eltávolítása** elemre. A **megjegyzések**, és a feladatátvételi teszttel kapcsolatos megfigyelések feljegyzéséhez mentéséhez. Ezzel a lépéssel törli a teszt feladatátvétele során létrehozott virtuális gépeket.

[További](site-recovery-test-failover-to-azure.md) vonatkozó feladatátvételi tesztet.


## <a name="vmware-account-permissions"></a>VMware fiókjához hozzárendelt jogosultságoktól

A Site Recovery VMware hozzáférésre van szüksége, a folyamatkiszolgálót a virtuális gépek automatikus észlelését, és feladatátvételi és a feladat-visszavételt a virtuális gépek.

- **Telepítse át**: csak áttelepíteni kívánt VMware virtuális gépek Azure-ba, anélkül, hogy legalább egyszer sikertelen vissza őket, ha használható a VMware-fiók egy csak olvasható szerepkör. Ilyen szerepkör feladatátvételi futtatható, de nem lehet leállítani a védett forrásgépek. Ez nem szükséges az áttelepítéshez.
- **Replikálás/Recover**: Ha a teljes replikáció (replikálja, feladatátvétel, feladat-visszavétel) telepíteni szeretné a fióknak kell lennie futtatható műveletek, például a létrehozásával és eltávolításával lemezek bekapcsolása a virtuális gépek stb.
- **Automatikus felderítés**: legalább egy csak olvasható fiókot kell megadni.


**Tevékenység** | **Szükséges fiók/szerepkör** | **Engedélyek** | **Részletek**
--- | --- | --- | ---
**A folyamatkiszolgáló automatikusan felderíti a VMware virtuális gépek** | Legalább egy írásvédett felhasználó van szüksége | Adatközpont objektum –> propagálása gyermekobjektum, a szerepkör = csak olvasható | Felhasználói datacenter szinten hozzárendelt, és rendelkezik hozzáféréssel az objektumokhoz az adatközpontban.<br/><br/> A hozzáférés korlátozása érdekében rendelje hozzá a **nem lehet hozzáférni** szerepkört a **gyermekre propagálása** objektum, a gyermekobjektumokra (vSphere-gazdagép, datastores, virtuális gépek és hálózatok).
**Feladatátvétel** | Legalább egy írásvédett felhasználó van szüksége | Adatközpont objektum –> propagálása gyermekobjektum, a szerepkör = csak olvasható | Felhasználói datacenter szinten hozzárendelt, és rendelkezik hozzáféréssel az objektumokhoz az adatközpontban.<br/><br/> A hozzáférés korlátozása érdekében rendelje hozzá a **nem lehet hozzáférni** szerepkört a **gyermekre propagálása** a gyermekobjektumokra (vSphere-gazdagép, datastores, virtuális gépek és hálózatok) objektum.<br/><br/> Akkor hasznos, ha az áttelepítési célokból, de nem a teljes replikáció, feladatátvétel, feladat-visszavétel.
**A feladatátvételi és a feladat-visszavétel** | Javasoljuk, hogy (Azure_Site_Recovery) szerepkör létrehozása a szükséges engedélyekkel, és hozzárendelheti a szerepkör egy VMware-felhasználó vagy csoport | Adatközpont objektum –> propagálása gyermekobjektum, a szerepkör = Azure_Site_Recovery<br/><br/> Datastore foglaljon le terület ->, keresse meg az adattároló, alacsony szintű fájlműveletek, távolítsa el a fájlt, frissítse a virtuális géphez tartozó fájlokat<br/><br/> Hálózat -> hálózat hozzárendelése<br/><br/> Erőforrás -> rendelje hozzá a virtuális gép erőforráskészlethez, energiaforrással rendelkező ki a virtuális gép áttelepítése, energiaforrással rendelkező, a virtuális gép áttelepítése<br/><br/> Feladatok -> hozzon létre feladat, a frissítési feladat<br/><br/> Virtuális gép konfigurációja -><br/><br/> Virtuálisgép -> kommunikáció -> válasz kérdést, az eszköz kapcsolat, a CD media konfigurálásához, a konfigurálásához a hajlékonylemezes adathordozót, kapcsolja ki, a bekapcsolás, VMware-eszközök telepítése<br/><br/> Virtuálisgép -> leltár -> létrehozási, regisztrálása, unregister<br/><br/> Virtuálisgép -> kiépítés engedélyezése virtuális gép letöltési ->, hogy a virtuálisgép-fájlok feltöltése<br/><br/> Virtuális gép pillanatkép ->-Remove pillanatképek > | Felhasználói datacenter szinten hozzárendelt, és rendelkezik hozzáféréssel az objektumokhoz az adatközpontban.<br/><br/> A hozzáférés korlátozása érdekében rendelje hozzá a **nem lehet hozzáférni** szerepkört a **gyermekre propagálása** objektum, a gyermekobjektumokra (vSphere-gazdagép, datastores, virtuális gépek és hálózatok).


## <a name="next-steps"></a>Következő lépések

Miután get replikációs, és fut, nem tervezett kimaradás esetén feladatátvételt az Azure-ba, és az Azure virtuális gépek jönnek létre a replikált adatokból. Érheti el munkaterheléseket és alkalmazásokat az Azure, amíg Ön nem az elsődleges helyre, amikor visszatér a normál működést.

- [További](site-recovery-failover.md) kapcsolatos különféle a feladatátvételeket, és futtatásukhoz.
- Ha a gépek replikálásához helyett és visszavétele, amelybe migrálna [további](site-recovery-migrate-to-azure.md#migrate-on-premises-vms-and-physical-servers).
- [További információ a feladat-visszavétel](site-recovery-failback-azure-to-vmware.md), a feladat-visszavételt és az Azure virtuális gépek replikálása az elsődleges helyszíni webhelyre.

## <a name="third-party-software-notices-and-information"></a>Harmadik felek szoftvereivel, megjegyzések és információk
Ne Localize vagy lefordítása

A szoftver- és a Microsoft termék vagy szolgáltatás futó belső vezérlőprogram alapján, vagy a szerződés magában foglalja a lenti projektek anyagok (együttesen "harmadik féltől származó kód").  A Microsoft a harmadik féltől származó kód nem eredeti szerzője.  Az eredeti szerzői és licencet, amely alatt a Microsoft az ilyen harmadik féltől származó kód kapott alábbi van beállítva.

A szakaszban található információk kapcsolatos alábbi projektek harmadik féltől származó kód-összetevőt. Ilyen licencek és információkat csak tájékoztatási célokat szolgálnak.  A harmadik féltől származó kód van folyamatban relicensed Önnek a Microsoft által a Microsoft szoftverfrissítések licencelési feltételeit a Microsoft termék vagy szolgáltatás alatt.  

B. szakaszban található információk kapcsolatos érhetőek Önnek a Microsoft által az eredeti licencfeltételek a harmadik felektől származó összetevőket.

A teljes fájl található a [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). A Microsoft kifejezetten át nem adott, az összes jogot fenntart által Ennek következménye, hogy belül vagy más módon.
