---
title: "Fizikai kiszolgálók replikálása az Azure-bA |} Microsoft Docs"
description: "Ismerteti, hogyan lehet telepíteni az Azure Site Recovery recoveryvel a replikáció, feladatátvétel és helyreállítás helyszíni windowsos/Linuxos fizikai kiszolgálók Azure az Azure portál használatával"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: physical-walkthrough-overview
ms.openlocfilehash: a9655ce1540c788d02d178eb619d2051cddda1c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
---
# <a name="replicate-physical-machines-to-azure-by-using-site-recovery"></a>Fizikai gépek replikálása az Azure Site Recovery segítségével


Ez a cikk ismerteti a helyszíni fizikai gépek replikálása az Azure-bA az Azure-portálon az Azure Site Recovery szolgáltatás használatával.

Ha azt szeretné, az Azure-ba (csak feladatátvételi) fizikai gépeket áttelepíteni, olvassa el a [áttelepítése az Azure Site Recovery szolgáltatással](site-recovery-migrate-to-azure.md) további.

Ez a cikk vagy a alsó megjegyzések és kérdések utáni a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites"></a>Előfeltételek

**Támogatási követelmények** | **Részletek**
--- | ---
**Azure** | Az [Azure-követelmények](site-recovery-prereq.md#azure-requirements) ismertetése.
**Helyszíni konfigurációs kiszolgáló** | A helyi számítógépen (fizikai vagy VMware virtuális gép) Windows Server 2012 R2 rendszerű vagy újabb. A konfigurációs kiszolgáló beállítása a Site Recovery üzembe helyezése során.<br/><br/> Alapértelmezés szerint a folyamatkiszolgáló és fő célkiszolgáló is települnek a számítógépen. Növelheti, ha szüksége lehet különálló folyamatkiszolgálót, és nem rendelkezik, ugyanazok a követelmények vonatkoznak, mint a konfigurációs kiszolgálót.<br/><br/> További tudnivalók az összetevők [a forráskörnyezet beállítása](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements).
**A helyszíni virtuális gépek** | Replikálni kívánt gépek rendszerűnek kell lennie egy [támogatott operációs rendszer](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) és adott a specifikációknak való [Azure-Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**URL-címek** | A konfigurációs kiszolgáló URL-hozzáférésre van szüksége:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Ha IP-címeken alapuló tűzfalszabályok szabály, akkor győződjön meg arról, hogy azok engedélyezik a kommunikációt az Azure-bA.<br/></br> Engedélyezze az [Azure-adatközpont IP-tartományait](https://www.microsoft.com/download/confirmation.aspx?id=41653) és a HTTPS-portot (443).<br/></br> Az IP-címtartományok (hozzáférés-vezérlés és identitás kezelésre szolgáló) USA nyugati régiója és az Azure-régió, az előfizetés engedélyezése.<br/><br/> Az URL-címet a MySQL Letöltés: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Mobilitási szolgáltatás** | Ez a szolgáltatás telepítve van, minden replikálni kívánt számítógépen.

## <a name="limitations"></a>Korlátozások

**A korlátozás** | **Részletek**
--- | ---
**Azure** | Tárolási és hálózati fiókok és a tárolónak ugyanabban a régióban kell lennie.<br/><br/> A prémium szintű storage-fiókok használatakor replikálási naplók tárolásához standard store-fiók is szükséges.<br/><br/> Közép-és Dél-Indiában premium-fiókok nem replikálja.
**Helyszíni konfigurációs kiszolgáló** | Ha a kiszolgáló telepíthető a VMware virtuális gépek, a virtuális gép hálózatiadapter-típus VMXNET3 kell lennie. Ha nem, [Ez a frissítés](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1).<br/><br/> Ha VMware virtuális gép használ, a vSphere PowerCLI 6.0 kell telepíteni.<br/><br> A gép nem lehet tartományvezérlő.<br/><br/> A gépnek rendelkeznie kell egy statikus IP-címet.<br/><br/> Az állomásnév 15 karakter lehet ne legyen, és az operációs rendszer angol nyelven kell lennie.
**Replikált gépek** | Győződjön meg arról [Azure virtuális gép korlátozások](site-recovery-prereq.md#azure-requirements).<br/><br/> Ha engedélyezi a virtuális Gépre kiterjedő konzisztencia, amely lehetővé teszi a gépek konzisztens adatpont együtt futtató ugyanaz az alkalmazás helyre kell állítani, nyissa meg a portot 20004 a számítógépen.<br/><br/> Adott típusú [Linux tárolási](site-recovery-support-matrix-to-azure.md#support-for-storage) támogatottak.
**Feladat-visszavétel** | Nem utasíthat el az Azure-ból a fizikai gép. Ha szeretné visszavenni a helyszíni feladatátvétel után, egy VMware-környezetben szüksége, hogy a VMware virtuális gép sikertelen lehet.


## <a name="set-up-azure"></a>Azure beállítása

1. [Állítsa be az Azure-hálózatot](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

      a. Azure virtuális gépek kerülnek ehhez a hálózathoz, ha feladatátvételt követően jönnek létre.

      b. Beállíthatja a hálózat az Azure-ban [erőforrás-kezelő](../resource-manager-deployment-model.md) vagy klasszikus módban.

2. Állítson be egy [Azure storage-fiók](../storage/storage-create-storage-account.md#create-a-storage-account) a replikált adatok tárolásához.

    a. A fiók lehet normál vagy [prémium](../storage/storage-premium-storage.md).

    b. Beállíthat egy fiókot a erőforrás-kezelő vagy a klasszikus módban.

## <a name="prepare-the-configuration-server"></a>A konfigurációs kiszolgáló előkészítése

1. Telepítse a Windows Server 2012 R2 vagy újabb egy helyszíni fizikai kiszolgáló vagy VMware virtuális gép.

2. Győződjön meg arról, hogy a gép számára elérhető legyen a felsorolt URL-címek [Előfeltételek](#prerequisites).

## <a name="prepare-for-mobility-service-installation"></a>Készítse elő a mobilitási szolgáltatás telepítése

Ha azt szeretné, amelyekkel a mobilitási szolgáltatást a fizikai gép, egy a gépek eléréséhez használható a folyamatkiszolgáló-fiók szükséges. A fiók csak a leküldéses telepítés szolgál. A tartományi vagy helyi fiók is használhatja:

  - A Windows Ha nem használ egy olyan tartományi fiók szeretné tiltsa le a távoli felhasználói hozzáférés-vezérlés a helyi számítógépen. Ehhez a rendszerleíró adatbázis **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, adja hozzá a DWORD bejegyzést **LocalAccountTokenFilterPolicy**, 1 értékű. Ha azt szeretné, a beállításjegyzék-bejegyzés hozzáadása a Windows parancssori felületen, írja be:

        ``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
  - A Linux a fióknak kell lennie a gyökér szintű felhasználó a forráskiszolgálón Linux.


## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="select-the-protection-goal"></a>Védelmi cél kiválasztása

Válassza ki, hogy mit szeretne replikálni, és hova.

1. Kattintson a **Recovery Services-tárolók** > **tároló**.
2. Az a **erőforrás** menüben kattintson a **Site Recovery** > **infrastruktúra előkészítése** > **védelmi cél** .

    ![Célok megválasztása](./media/site-recovery-vmware-to-azure/choose-goal-physical.PNG)

3. A **védelmi cél**, jelölje be **az Azure-bA** > **nem virtualizált vagy egyéb**.


## <a name="set-up-the-source-environment"></a>A forráskörnyezet beállítása

Állítsa be a konfigurációs kiszolgáló, és regisztrálja őket a tárolóban lévő virtuális gépek felderítése.

1. Kattintson a **helyreállítási hely** > **infrastruktúra előkészítése** > **forrás**.
2. Ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló**.

    ![A forrás beállítása](./media/site-recovery-vmware-to-azure/set-source1.png)

3. A **kiszolgáló hozzáadása**, ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.
4. Töltse le a **Site Recovery az egységes telepítő** telepítőfájlt.
5. Töltse le a **tárolóbeli regisztrációs kulcs**. Az egységes telepítő kell ezt a kulcsot. A kulcs a generálásától számított öt napig érvényes.

   ![A forrás beállítása](./media/site-recovery-vmware-to-azure/set-source2.png)


## <a name="run-site-recovery-unified-setup"></a>Futtassa a Site Recovery egyesített telepítő

Mielőtt elkezdené, tegye a következőket:

- Gyorsan áttekintheti videó. (A videó ismerteti, hogyan lehet VMware virtuális gépek replikálása, de a folyamat hasonló a fizikai gép replikálása.)

    > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

- A konfigurációs kiszolgáló számítógépen győződjön meg arról, hogy a rendszer órája szinkronizálva van a [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Ha az előtérben 15 perc vagy mögött található, a telepítés meghiúsulhat.
- A konfigurációs kiszolgáló számítógépen futtassa a telepítőt helyi rendszergazdaként.
- Győződjön meg arról, hogy a TLS 1.0 engedélyezve van a számítógépen.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> A konfigurációs kiszolgáló is telepíthető [a parancssorból](http://aka.ms/installconfigsrv).


## <a name="set-up-the-target-environment"></a>A célkörnyezet beállítása

A célkörnyezet beállítása előtt ellenőrizze, hogy, hogy rendelkezik egy [Azure storage-fiók és a hálózati](#set-up-azure).

1. Kattintson a **infrastruktúra előkészítése** > **cél**, és válassza ki a használni kívánt Azure-előfizetést.
2. Adja meg, hogy a tároló üzembe helyezési modellben erőforrás-kezelő vagy a klasszikus.
3. A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure storage-fiókok és a hálózatok.

   ![cél](./media/site-recovery-vmware-to-azure/gs-target.png)

4. Ha még nem hozott létre a storage-fiók vagy a hálózaton, kattintson a **+ tárfiók** vagy **+ hálózat** Resource Manager-fiók létrehozása vagy szövegközi hálózati.

## <a name="set-up-replication-settings"></a>Replikációs beállítások megadása

Mielőtt elkezdené, gyorsan áttekintheti videó. (A videó ismerteti, hogyan lehet VMware virtuális gépek replikálása, de a folyamat hasonló a fizikai gép replikálása.)

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. Hozzon létre egy új replikációs házirendet, kattintson a **Site Recovery-infrastruktúra** > **replikációs házirendek** > **+ replikációs házirend**.
2. A **replikációs házirend létrehozása**, megadhatja a házirend nevét.
3. Az **RPO küszöbértéke** beállításnál adja meg az RPO-korlátot. Ez az érték határozza meg, milyen gyakran adatok helyreállítási pontot hoz létre. Riasztás akkor jön létre, ha a folyamatos replikálásra túllépi ezt a korlátozást.
4. A **helyreállításipont-megőrzést**, adja meg (órában) mennyi az adatmegőrzési időtartam legyen az egyes helyreállítási pontok. A replikált virtuális gépek bármelyik pontra ablakban állíthatók helyre. Akár 24 óra megőrzési prémium szintű Storage replikált gépek esetén támogatott. Akár 72 órát megőrzési szabványos tárhelyre replikált gépek esetén támogatott.
5. A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg, milyen gyakran (percben) alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontok jönnek létre. Kattintson a **OK** a szabályzat létrehozásához.

    ![Replikációs szabályzat](./media/site-recovery-vmware-to-azure/gs-replication2.png)

6. Ha egy új házirendet hoz létre, akkor automatikusan a konfigurációs kiszolgáló társítja. Alapértelmezés szerint a megfelelő házirend automatikusan létrejön a feladat-visszavételre. Például, ha a replikációs házirend **rep-házirend**, akkor a feladat-visszavétel házirend **rep-házirend-feladat-visszavétel**. Ez a házirend nem használ, amíg nem indítja el a feladat-visszavétel az Azure-ból.  


## <a name="plan-capacity"></a>Kapacitástervezés

1. Most, hogy az alapvető infrastruktúra beállítása, gondolja át a kapacitástervezés és a mérje fel, hogy mikor szükséges további erőforrások. [További információk](site-recovery-plan-capacity-vmware.md).
2. Amikor elkészült, a kapacitástervezés, válassza ki a **Igen** a **elvégezte a kapacitástervezést?**

   ![Kapacitástervezés](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)


## <a name="prepare-vms-for-replication"></a>Készítse elő a virtuális gépek a replikáció

Minden replikálni kívánt gépek rendelkeznie kell a mobilitási szolgáltatás telepítve van. A mobilitási szolgáltatás számos módon telepíthető:

- Telepítse a szolgáltatást egy leküldéses telepítési adatok a folyamatkiszolgálótól. Elő kell készíteni a gépek előre ezt a módszert használja.
- A szolgáltatás telepítéséhez használja a központi telepítési eszközök, például a System Center Configuration Manager vagy az Azure Automation célállapot-konfiguráció.
- Manuálisan telepítse a szolgáltatást.

[További információk](site-recovery-vmware-to-azure-install-mob-svc.md).


## <a name="enable-replication"></a>A replikáció engedélyezése

Előkészületek:

- Egyes rendelkeznie kell Azure felhasználói fiókja [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) engedélyezni szeretné egy új virtuális gép az Azure-bA replikálását.
- Amikor hozzá, vagy módosítsa a virtuális gépek, is eltarthat, és 15 perc vagy már a módosítások érvénybe lépéséhez, és hogy megjelenjenek a portálon.
- A legutóbb felfedezett ellenőrizze, hogy a virtuális gépek **konfigurációs kiszolgálók** > **utolsó lépjen kapcsolatba a**.
- Az ütemezett felderítési várakozás nélkül adja hozzá a virtuális gépeket, jelölje ki a konfigurációs kiszolgálót (ne kattintson azt), és kattintson **frissítése**.
- A virtuális gépek kész az ügyfélleküldéses telepítést, ha a folyamatkiszolgáló automatikusan telepíti a mobilitási szolgáltatást, amikor a replikáció engedélyezése.
- Gyorsan áttekintheti videó. (A videó ismerteti, hogyan lehet VMware virtuális gépek replikálása, de a folyamat hasonló a fizikai gép replikálása.)

    >[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]


### <a name="exclude-disks-from-replication"></a>Lemezek kizárása a replikációból

Alapértelmezés szerint a rendszer a gép minden lemezét replikálja. Lemezek lehet kizárni a replikálásból. Például előfordulhat, hogy nem kívánt lemezek, amelyek ideiglenes adatok replikálása, vagy frissítette egyes adatok újraindításakor egy gép vagy alkalmazások (például pagefile.sys vagy SQL Server tempdb).

### <a name="replicate-vms"></a>Virtuális gépek replikálása

1. Kattintson a **alkalmazás replikálása** > **forrás**.
2. A **forrás**, jelölje be **helyszíni**.
3. A **adatforrásról**, válassza ki a konfigurációs kiszolgáló nevét.
4. A **számítógép típusú**, jelölje be **fizikai gépek**.
5. A **folyamatkiszolgáló**, válassza a folyamatkiszolgáló. Ha még nem hozott létre további folyamat kiszolgálókat, ehhez a kiszolgálóhoz a konfigurációs kiszolgáló. Ezután kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/site-recovery-physical-to-azure/chooseVM.png)

6. A **cél**, jelölje be a **előfizetés** és a **erőforráscsoport** található, amely szeretne létrehozni az Azure virtuális gépek a feladatátvételt követően. Válassza ki az Azure-ban használni kívánt telepítési modelljét (klasszikus vagy erőforrás-kezelő) a átvevő virtuális gépekre vonatkozóan.

7. Válassza ki az adatok replikálásához használni kívánt Azure-tárfiókot. Ha nem szeretné, már létrehozott fiók használatára, létrehozhat egy újat.

8. Válassza ki a **Azure hálózati** és **alhálózati** amelyhez Azure virtuális gépek a feladatátvételt követően kapcsolódni. Ha a megadott hálózati beállításokat az összes védelemre kijelölt gépre szeretné alkalmazni, válassza a **Beállítás most a kijelölt gépekhez** lehetőséget. Ha az egyes gépeknél külön-külön szeretné beállítani az Azure-hálózatot, kattintson a **Beállítás később** elemre. Ha nem szeretné használni a meglévő hálózat, létrehozhat egyet.

    ![A replikáció engedélyezése](./media/site-recovery-physical-to-azure/targetsettings.png)

9. A **fizikai gépek**, kattintson a **+ a fizikai gép** , és írja be a **neve** és **IP-cím**. Válassza ki a replikálni kívánt gépek operációs rendszerének. Amíg a számítógépek felderítése és a listában szereplő néhány percet vesz igénybe.

    ![A replikáció engedélyezése](./media/site-recovery-physical-to-azure/machineselect.png)

10. A **tulajdonságok** > **tulajdonságainak konfigurálása**, jelölje ki a fiókot használják a folyamatkiszolgáló automatikusan telepíteni a mobilitási szolgáltatás a számítógépen.
11. Alapértelmezés szerint a rendszer replikálja az összes lemez. Kattintson a **összes lemez**, és törölje a lemezek nem szeretne replikálni. Ezután kattintson az **OK** gombra. Virtuális gép további tulajdonságokat később állíthatja be.

    ![A replikáció engedélyezése](./media/site-recovery-physical-to-azure/configprop.png)

12. A **replikációs beállítások** > **replikáció beállításainak konfigurálása**, győződjön meg arról, hogy a megfelelő replikációs házirend van-e kiválasztva. Ha módosít egy házirendet, a változások történnek a replikáló gépek és új gépre.
13. Engedélyezése **virtuális Gépre kiterjedő konzisztencia** Ha azt szeretné, hogy gyűjtse össze a gépet egy replikációs csoporthoz, és adja meg a csoport nevét. Ezután kattintson az **OK** gombra. Vegye figyelembe:

    a. Egy replikációs csoportnak tagjai gépek replikálása együtt, és ha azok feladatátvételt megosztott összeomlás-konzisztens és alkalmazáskonzisztens helyreállítási pontokat.

    b. Azt javasoljuk, hogy gyűjtse össze a virtuális gépek és fizikai kiszolgálók együtt, hogy látni lehessen a munkaterhelések. Több virtuális Gépre kiterjedő konzisztencia engedélyezése hatással lehet a számítási feladat teljesítményére. Akkor kell használni, csak akkor, ha a gépeken ugyanaz az alkalmazás fut, és konzisztenciára van szükség.

    ![A replikáció engedélyezése](./media/site-recovery-physical-to-azure/policy.png)

14. Kattintson a **engedélyezze a replikálást**. Előrehaladásának nyomon követheti a **Védelemengedélyezési** feladat **beállítások** > **feladatok** > **Site Recovery-feladatok**. A **Védelem véglegesítése** feladat befejeződését követően a gép készen áll a feladatátvételre.

Replikáció engedélyezése után a mobilitási szolgáltatás telepítve van, ha ügyfélleküldéses telepítést. Miután a mobilitási szolgáltatás leküldéses gépen telepítve, egy védelmi feladatot elindul, és sikertelen lesz. A meghibásodás után meg kézzel kell újraindítania az egyes. Majd a védelmi feladat újra kezdődik, és a kezdeti replikálást.


### <a name="view-and-manage-azure-vm-properties"></a>Megtekintése és kezelése az Azure virtuális gép tulajdonságai

Azt javasoljuk, hogy ellenőrizze a virtuális gép tulajdonságait, és hajtsa végre a szükséges módosításokat.

1. Kattintson a **replikált elemek**, és jelölje ki a gépet. A **Essentials** panel gépek beállítások és állapotával kapcsolatos adatokat jeleníti meg.
2. A **Tulajdonságok** résznél tekintheti meg a virtuális gép replikációs és feladatátvételi adatait.
3. A **Számítás és hálózat** > **Számítási tulajdonságok** résznél adhatja meg az Azure virtuális gép nevét és a cél méretét. Ha szükséges, írja át úgy a nevet, hogy az megfeleljen az [Azure-követelményeknek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
4. A célhálózat alhálózatot és IP-cím az Azure virtuális Géphez rendelt beállításainak módosítása:

    a. A cél IP-címe beállítható.

    b.  Ha nem ad meg egy címet, a átvevő gép DHCP használja.

    c. Ha egy címet, amely nem használható feladatátadásra, feladatátvétel nem működik.

    d. A cél IP-címe feladatátvételi tesztre is használható, amennyiben a cím elérhető a feladatátvételi teszt hálózatában.

    e. A hálózati adapterek számát adja meg, ha a cél virtuális gép méretét határozza meg:

     - Ha a forrásgépen hálózati adapterek száma azonos, vagy kisebb mint a célgép mérete, akkor a cél engedélyezett adapterek száma a ugyanannyi adapter rendelkezik, mint a forrás.
     - Ha a forrás virtuális gépek adaptereinek száma meghaladja a célmérethez engedélyezett maximumot, a rendszer a célmérethez engedélyezett maximális számot használja.
     - Például ha a forrásgépen két hálózati adaptert, és a célgép mérete négy támogatja, a célgépen két adapter rendelkezik. Ha a forrásgépen két adapter, de a támogatott célméretet támogatja a csak egy, a célgépen csak egy adapterrel rendelkezik.     
   - Ha a virtuális gépnek több hálózati adapter, minden csatlakoznak az ugyanazon a hálózaton.
   - Ha a virtuális gépnek több hálózati adaptert, majd az elsőt a listán látható lesz a *alapértelmezett* az Azure virtuális gép hálózati adapteréhez.
5. A **lemezek**, láthatja, hogy a virtuális gép operációs rendszer, és az adatok lemezek, amelyek replikálódnak.

## <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása

Miután beállította mindent, győződjön meg arról, hogy minden a várt módon működik a feladatátvételi teszt futtatása. Tekintse meg a gyors áttekintő videó elindítása előtt.

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]


1. Az egyetlen számítógépen, feladatátvételt **beállítások** > **replikált elemek**, kattintson a **feladatátvételi teszt**.

    ![A feladatátvételi teszt](./media/site-recovery-vmware-to-azure/TestFailover.png)

2. Helyreállítási terv feladatátadásához a **Beállítások** > **Helyreállítási tervek** menüben kattintson jobb gombbal a kívánt tervre, majd válassza a **Feladatátvételi teszt** lehetőséget. Helyreállítási terv létrehozásához [kövesse ezeket az utasításokat](site-recovery-create-recovery-plans.md).  
3. A **feladatátvételi teszt**, válassza ki az Azure-hálózatot, amelyhez Azure virtuális gépek csatlakoznak a feladatátvételt követően.
4. A feladatátvételi művelet elindításához kattintson az **OK** gombra. Előrehaladásának kattintva vagy a virtuális gép tulajdonságainak megnyitásához kattintson a **feladatátvételi teszt** feladatot a tároló neve > **beállítások** > **feladatok**  >  **Site Recovery-feladatok**.
5. A feladatátvétel befejezése után meg kell tudni a replika Azure machine jelennek meg az Azure-portálon > **virtuális gépek**. Győződjön meg arról, hogy a virtuális gép a megfelelő, csatlakozik-e a megfelelő hálózati méretét, és fut-e.
6. Ha [elvégezte a feladatátvételt követő csatlakozáshoz szükséges előkészületeket](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover), most tudnia kell csatlakozni az Azure virtuális géphez.
7. Miután elkészült, kattintson **karbantartása a feladatátvételi teszt** a a helyreállítási terv. A **megjegyzések**, és a feladatátvételi teszttel kapcsolatos megfigyelések feljegyzéséhez mentéséhez. Ezzel a lépéssel törli a virtuális gép teszt feladatátvétele során létrehozott.

További információkért lásd: a [feladatátvételi teszt Azure-bA](site-recovery-test-failover-to-azure.md) dokumentum.

## <a name="next-steps"></a>Következő lépések

Miután get replikációs, és fut, nem tervezett kimaradás esetén feladatátvételt az Azure-ba, és az Azure virtuális gépek jönnek létre a replikált adatokból. Érheti el munkaterheléseket és alkalmazásokat az Azure, amíg Ön nem az elsődleges helyre, amikor visszatér a normál működést.

- [További információk](site-recovery-failover.md) a feladatátvételek különféle típusaival és a futtatásukkal kapcsolatban.
- Ha a gépek replikálásához helyett és visszavétele, amelybe migrálna [további](site-recovery-migrate-to-azure.md#migrate-on-premises-vms-and-physical-servers).
- Ha fizikai gépeket replikál, akkor csak a feladat-visszavétel VMware környezetben. [További információ a feladat-visszavétel](site-recovery-failback-azure-to-vmware.md).

## <a name="third-party-software-notices-and-information"></a>Harmadik felek szoftvereivel, megjegyzések és információk
Ne Localize vagy lefordítása

A szoftver- és a Microsoft termék vagy szolgáltatás futó belső vezérlőprogram alapján, vagy a szerződés magában foglalja a lenti projektek anyagok (együttesen "külső kód"). A Microsoft nem az eredeti szerző, a külső kódját. Az eredeti szerzői és licencet, amely alatt a Microsoft külső kódok, kapott alábbi van beállítva.

A szakaszban található információk külső kód összetevőt az alábbi projektek kapcsolódik. Ilyen licencek és információkat csak tájékoztatási célokat szolgálnak. A külső kód van folyamatban relicensed Önnek a Microsoft által a Microsoft szoftverfrissítések licencelési feltételeit a Microsoft termék vagy szolgáltatás alatt.  

B. szakaszban található információk kapcsolatos érhetőek Önnek a Microsoft által az eredeti licencfeltételek a harmadik felektől származó összetevőket.

A teljes fájl megtalálható a [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). A Microsoft kifejezetten át nem adott, az összes jogot fenntart által Ennek következménye, hogy belül, vagy más módon.
