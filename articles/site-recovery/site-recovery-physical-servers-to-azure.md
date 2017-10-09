---
title: "fizikai kiszolgálók tooAzure aaaReplicate |} Microsoft Docs"
description: "Ismerteti, hogyan toodeploy Azure Site Recovery tooorchestrate replikációs, feladatátvételének és helyreállításának helyszíni windowsos/Linuxos fizikai kiszolgálók tooAzure hello Azure-portál használatával"
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
ms.openlocfilehash: cf5928fb631f6858d57b27f6f21babc312714e21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
---
# <a name="replicate-physical-machines-tooazure-by-using-site-recovery"></a>Fizikai gépek tooAzure replikálásához a Site Recovery segítségével


A cikkből megtudhatja, hogyan tooreplicate a helyszíni fizikai gépek tooAzure hello Azure Site Recovery szolgáltatás hello Azure-portál használatával.

Ha azt szeretné, toomigrate fizikai gépek tooAzure (csak feladatátvételi), olvasási [telepítse át a Site Recovery szolgáltatással tooAzure](site-recovery-migrate-to-azure.md) további toolearn.

Ez a cikk vagy hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites"></a>Előfeltételek

**Támogatási követelmények** | **Részletek**
--- | ---
**Azure** | Az [Azure-követelmények](site-recovery-prereq.md#azure-requirements) ismertetése.
**Helyszíni konfigurációs kiszolgáló** | A helyi számítógépen (fizikai vagy VMware virtuális gép) Windows Server 2012 R2 rendszerű vagy újabb. Hello konfigurációs kiszolgáló beállítása a Site Recovery üzembe helyezése során.<br/><br/> Alapértelmezés szerint hello folyamatkiszolgáló és fő célkiszolgáló is települnek a számítógépen. Vertikális felskálázás, előfordulhat, hogy különálló folyamatkiszolgálót, és rendelkezik hello ugyanazok vonatkoznak, mint hello konfigurációs kiszolgáló.<br/><br/> További tudnivalók az összetevők [hello forráskörnyezet beállítása](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements).
**A helyszíni virtuális gépek** | Gépeknek meg tooreplicate rendszerűnek kell lennie egy [támogatott operációs rendszer](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) és adott a specifikációknak való [Azure-Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**URL-címek** | meg kell hello konfigurációs kiszolgálót toothese URL hozzáférés:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Ha IP-címeken alapuló tűzfalszabályok szabály, akkor győződjön meg arról, hogy azok engedélyezik a kommunikációt tooAzure.<br/></br> Hello engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653) és hello HTTPS (443) port.<br/></br> Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés és az USA nyugati régiója (hozzáférés-vezérlés és identitás kezelésre szolgáló).<br/><br/> Engedélyezi a hello MySQL letöltési URL-címe: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Mobilitási szolgáltatás** | Ez a szolgáltatás tooreplicate kívánt minden egyes számítógépen.

## <a name="limitations"></a>Korlátozások

**A korlátozás** | **Részletek**
--- | ---
**Azure** | Tárolási és hálózati fiókoknak kell lenniük a hello és hello-tárolónak ugyanabban a régióban.<br/><br/> A prémium szintű storage-fiókok használatához is szükség van egy standard fiók toostore replikálási naplók tárolásához.<br/><br/> Közép-és Dél-Indiában toopremium fiókok nem replikálja.
**Helyszíni konfigurációs kiszolgáló** | Ha VMware virtuális gép hello konfigurációs kiszolgálón telepíti, virtuális gép hálózatiadapter-típus hello VMXNET3 kell lennie. Ha nem, [Ez a frissítés](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1).<br/><br/> Ha VMware virtuális gép használ, a vSphere PowerCLI 6.0 kell telepíteni.<br/><br> hello gépet nem lehet tartományvezérlő.<br/><br/> hello gépnek rendelkeznie kell egy statikus IP-címet.<br/><br/> hello állomásnevet kell 15 karakter vagy kevesebb, és hello operációs rendszernek kell lennie, angol nyelven.
**Replikált gépek** | Győződjön meg arról [Azure virtuális gép korlátozások](site-recovery-prereq.md#azure-requirements).<br/><br/> Ha azt szeretné, hogy tooenable virtuális Gépre kiterjedő konzisztencia, amely lehetővé teszi a munkaterhelés ugyanazon toobe helyre hello futtató gépek együtt tooa azonos adatokat mutasson, nyissa meg a portot 20004 hello gépen.<br/><br/> Adott típusú [Linux tárolási](site-recovery-support-matrix-to-azure.md#support-for-storage) támogatottak.
**Feladat-visszavétel** | Nem utasíthat el újból az Azure tooa fizikai gépen. Ha a feladatátvételt követően toobe képes toofail hátsó tooon helyszíni, VMware környezetben szüksége, hogy a háttérben tooa VMware virtuális gép sikertelen lehet.


## <a name="set-up-azure"></a>Azure beállítása

1. [Állítsa be az Azure-hálózatot](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

      a. Azure virtuális gépek kerülnek ehhez a hálózathoz, ha feladatátvételt követően jönnek létre.

      b. Beállíthatja a hálózat az Azure-ban [erőforrás-kezelő](../resource-manager-deployment-model.md) vagy klasszikus módban.

2. Állítson be egy [Azure storage-fiók](../storage/storage-create-storage-account.md#create-a-storage-account) a replikált adatok tárolásához.

    a. hello fiók is lehet standard vagy [prémium](../storage/storage-premium-storage.md).

    b. Beállíthat egy fiókot a erőforrás-kezelő vagy a klasszikus módban.

## <a name="prepare-hello-configuration-server"></a>Hello konfigurációs kiszolgáló előkészítése

1. Telepítse a Windows Server 2012 R2 vagy újabb egy helyszíni fizikai kiszolgáló vagy VMware virtuális gép.

2. Ellenőrizze, hogy hello gépnek toohello URL-címek felsorolt [Előfeltételek](#prerequisites).

## <a name="prepare-for-mobility-service-installation"></a>Készítse elő a mobilitási szolgáltatás telepítése

Ha azt szeretné, hogy toopush hello mobilitási szolgáltatás tooa fizikai gép, egy hello folyamat server tooaccess hello gépek által használt fiók szükséges. hello fiók csak hello ügyfélleküldéses telepítés használatos. A tartományi vagy helyi fiók is használhatja:

  - A Windows Ha nem használ egy olyan tartományi fiók kell toodisable távelérési felhasználói vezérlő hello helyi számítógépen. toodo ez hello rendszerleíró adatbázis **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, adjon hozzá hello DWORD bejegyzést **LocalAccountTokenFilterPolicy**, 1 értékű. Ha tooadd hello bejegyzés Windows parancssori felületen, írja be:

        ``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
  - Linux a hello fióknak kell lennie a gyökér szintű felhasználó hello Linux forráskiszolgálón.


## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="select-hello-protection-goal"></a>Hello védelmi cél kiválasztása

Válassza ki a kívánt tooreplicate és a tooreplicate, ahová.

1. Kattintson a **Recovery Services-tárolók** > **tároló**.
2. A hello **erőforrás** menüben kattintson a **Site Recovery** > **infrastruktúra előkészítése** > **védelmi cél**.

    ![Célok megválasztása](./media/site-recovery-vmware-to-azure/choose-goal-physical.PNG)

3. A **védelmi cél**, jelölje be **tooAzure** > **nem virtualizált vagy egyéb**.


## <a name="set-up-hello-source-environment"></a>Hello forráskörnyezet beállítása

Hello konfigurációs kiszolgáló, regisztrálja azt hello tárolóban, és virtuális gépek felderítése.

1. Kattintson a **helyreállítási hely** > **infrastruktúra előkészítése** > **forrás**.
2. Ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló**.

    ![A forrás beállítása](./media/site-recovery-vmware-to-azure/set-source1.png)

3. A **kiszolgáló hozzáadása**, ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.
4. Töltse le a hello **Site Recovery az egységes telepítő** telepítőfájlt.
5. Töltse le a hello **tárolóbeli regisztrációs kulcs**. Az egységes telepítő kell ezt a kulcsot. hello kulcs a generálásától öt napig esetén érvényes.

   ![A forrás beállítása](./media/site-recovery-vmware-to-azure/set-source2.png)


## <a name="run-site-recovery-unified-setup"></a>Futtassa a Site Recovery egyesített telepítő

Mielőtt elkezdené, hello a következő:

- Gyorsan áttekintheti videó. (hello videó bemutatja, hogyan tooreplicate VMware virtuális gépek, de hello feldolgozni hasonló a fizikai gép replikálása.)

    > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

- A konfigurációs kiszolgáló számítógépén hello, győződjön meg arról, hogy hello rendszeróra szinkronizálva van a [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Ha az előtérben 15 perc vagy mögött található, a telepítés meghiúsulhat.
- Futtassa a telepítőt helyi rendszergazdaként a hello konfigurációs kiszolgáló számítógépén.
- Győződjön meg arról, hogy a TLS 1.0 hello gépen engedélyezve van.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> hello konfigurációs kiszolgálón is telepíthető [hello parancssorból](http://aka.ms/installconfigsrv).


## <a name="set-up-hello-target-environment"></a>Hello célkörnyezet beállítása

Hello célkörnyezet beállítása előtt ellenőrizze, hogy rendelkezik toomake egy [Azure storage-fiók és a hálózati](#set-up-azure).

1. Kattintson a **infrastruktúra előkészítése** > **cél**, és válassza ki a kívánt toouse Azure-előfizetés hello.
2. Adja meg, hogy a tároló üzembe helyezési modellben erőforrás-kezelő vagy a klasszikus.
3. A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure storage-fiókok és a hálózatok toomake.

   ![cél](./media/site-recovery-vmware-to-azure/gs-target.png)

4. Ha még nem hozott létre a storage-fiók vagy a hálózaton, kattintson a **+ tárfiók** vagy **+ hálózat** toocreate egy erőforrás-kezelő fiók vagy a hálózati beágyazott.

## <a name="set-up-replication-settings"></a>Replikációs beállítások megadása

Mielőtt elkezdené, gyorsan áttekintheti videó. (hello videó bemutatja, hogyan tooreplicate VMware virtuális gépek, de hello feldolgozni hasonló a fizikai gép replikálása.)

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. egy új replikációs házirendet toocreate kattintson **Site Recovery-infrastruktúra** > **replikációs házirendek** > **+ replikációs házirend**.
2. A **replikációs házirend létrehozása**, megadhatja a házirend nevét.
3. A **helyreállítási Időkorlát küszöbértéke**, adja meg a hello RPO korlátot. Ez az érték határozza meg, milyen gyakran adatok helyreállítási pontot hoz létre. Riasztás akkor jön létre, ha a folyamatos replikálásra túllépi ezt a korlátozást.
4. A **helyreállításipont-megőrzést**, adja meg (órában) mennyi ideig áll hello adatmegőrzési időtartam az egyes helyreállítási pontok. A replikált virtuális gépek lehet helyreállítani tooany pont ablakban. Mentése too24 órányi megőrzési gépek replikált toopremium tárolás esetén támogatott. Mentése too72 órányi megőrzési gépek replikált toostandard tárolás esetén támogatott.
5. A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg, milyen gyakran (percben) alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontok jönnek létre. Kattintson a **OK** toocreate hello házirend.

    ![Replikációs szabályzat](./media/site-recovery-vmware-to-azure/gs-replication2.png)

6. Ha egy új házirendet hoz létre, azt automatikusan hello konfigurációs kiszolgáló társítja. Alapértelmezés szerint a megfelelő házirend automatikusan létrejön a feladat-visszavételre. Például ha hello replikációs házirend nem **rep-házirend**, akkor hello feladatátvételi házirendhez **rep-házirend-feladat-visszavétel**. Ez a házirend nem használ, amíg nem indítja el a feladat-visszavétel az Azure-ból.  


## <a name="plan-capacity"></a>Kapacitástervezés

1. Most, hogy az alapvető infrastruktúra beállítása, gondolja át a kapacitástervezés és a mérje fel, hogy mikor szükséges további erőforrások. [További információk](site-recovery-plan-capacity-vmware.md).
2. Amikor elkészült, a kapacitástervezés, válassza ki a **Igen** a **elvégezte a kapacitástervezést?**

   ![Kapacitástervezés](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)


## <a name="prepare-vms-for-replication"></a>Készítse elő a virtuális gépek a replikáció

Minden számítógép, amelyet az tooreplicate rendelkeznie kell hello mobilitási szolgáltatás telepítve van. Hello mobilitásiszolgáltatás többféle módszerrel is telepíthető:

- Hello szolgáltatás telepítése hello folyamat kiszolgálóról leküldéses telepítés. Előzetes toouse tooprepare gépek kell ezt a módszert.
- Hello szolgáltatás telepítéséhez használja a központi telepítési eszközök, például a System Center Configuration Manager vagy az Azure Automation célállapot-konfiguráció.
- Telepítse manuálisan a hello szolgáltatást.

[További információk](site-recovery-vmware-to-azure-install-mob-svc.md).


## <a name="enable-replication"></a>A replikáció engedélyezése

Előkészületek:

- Az Azure felhasználói fiókot kell toohave bizonyos [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) egy új virtuális gép tooAzure tooenable replikációját.
- Amikor hozzá, vagy módosítsa a virtuális gépek, is igénybe vehet fel too15 perc vagy már módosítások tootake hatás és a számukra tooappear hello portálon.
- A legutóbb felfedezett hello idő ellenőrizze, hogy a virtuális gépek **konfigurációs kiszolgálók** > **utolsó lépjen kapcsolatba a**.
- virtuális gépek tooadd hello ütemezett felderítési, kiemelési hello konfigurációs kiszolgáló várakozás nélkül (ne kattintson azt), és kattintson a **frissítése**.
- A virtuális gépek kész az ügyfélleküldéses telepítést, ha a hello folyamatkiszolgáló automatikusan telepíti az hello mobilitási szolgáltatás, amikor a replikáció engedélyezése.
- Gyorsan áttekintheti videó. (hello videó bemutatja, hogyan tooreplicate VMware virtuális gépek, de hello feldolgozni hasonló a fizikai gép replikálása.)

    >[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]


### <a name="exclude-disks-from-replication"></a>Lemezek kizárása a replikációból

Alapértelmezés szerint a rendszer a gép minden lemezét replikálja. Lemezek lehet kizárni a replikálásból. Például előfordulhat, hogy nem kívánja tooreplicate lemezek ideiglenes adatokkal, vagy frissítette egyes adatok újraindításakor egy gép vagy alkalmazások (például pagefile.sys vagy SQL Server tempdb).

### <a name="replicate-vms"></a>Virtuális gépek replikálása

1. Kattintson a **alkalmazás replikálása** > **forrás**.
2. A **forrás**, jelölje be **helyszíni**.
3. A **adatforrásról**, jelölje be hello konfigurációs kiszolgáló nevét.
4. A **számítógép típusú**, jelölje be **fizikai gépek**.
5. A **folyamatkiszolgáló**, válassza a folyamatkiszolgáló hello. Ha még nem hozott létre további folyamat kiszolgálókat, ehhez a kiszolgálóhoz hello konfigurációs kiszolgáló. Ezután kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/site-recovery-physical-to-azure/chooseVM.png)

6. A **cél**, jelölje be hello **előfizetés** és hello **erőforráscsoport** használni kívánt toocreate hello Azure virtuális gépek a feladatátvételt követően. Válassza ki a hello telepítési modell, amelyet az Azure-ban toouse (klasszikus vagy erőforrás-kezelő) a hello átvevő virtuális gépek.

7. Válassza ki a kívánt toouse adatok replikálásához hello Azure storage-fiók. Ha már beállította fiók toouse nem szeretné, létrehozhat egy újat.

8. Jelölje be hello **Azure hálózati** és **alhálózati** toowhich Azure virtuális gépek a feladatátvételt követően csatlakozhatnak. Válassza ki **beállítás most a kijelölt gépekhez** tooapply hello beállítás tooall számítógépek választja a védelem. Válassza ki **konfigurálását később** tooselect hello gépenként Azure-hálózatot. Ha egy meglévő hálózati toouse nem szeretné, akkor hozzon létre egyet.

    ![A replikáció engedélyezése](./media/site-recovery-physical-to-azure/targetsettings.png)

9. A **fizikai gépek**, kattintson a **+ a fizikai gép** , és írja be a hello **neve** és **IP-cím**. Válassza ki az operációs rendszer hello hello gép tooreplicate szeretné. Amíg a számítógépek felderítése és hello listán látható néhány percet vesz igénybe.

    ![A replikáció engedélyezése](./media/site-recovery-physical-to-azure/machineselect.png)

10. A **tulajdonságok** > **tulajdonságainak konfigurálása**, jelölje be hello fiók hello folyamat server tooautomatically által használt toobe hello mobilitási szolgáltatás telepítése hello gépen.
11. Alapértelmezés szerint a rendszer replikálja az összes lemez. Kattintson a **összes lemez**, és törölje a nem kívánt tooreplicate lemezzel. Ezután kattintson az **OK** gombra. Virtuális gép további tulajdonságokat később állíthatja be.

    ![A replikáció engedélyezése](./media/site-recovery-physical-to-azure/configprop.png)

12. A **replikációs beállítások** > **replikáció beállításainak konfigurálása**, győződjön meg arról, hogy helyes-e replikációs házirend be van jelölve hello. Ha módosít egy házirendet, a változtatások alkalmazott toohello gép és toonew gépek replikálásához.
13. Engedélyezése **virtuális Gépre kiterjedő konzisztencia** Ha toogather gépeihez egy replikációs csoporthoz, és adjon meg egy nevet hello csoportnak. Ezután kattintson az **OK** gombra. Vegye figyelembe:

    a. Egy replikációs csoportnak tagjai gépek replikálása együtt, és ha azok feladatátvételt megosztott összeomlás-konzisztens és alkalmazáskonzisztens helyreállítási pontokat.

    b. Azt javasoljuk, hogy gyűjtse össze a virtuális gépek és fizikai kiszolgálók együtt, hogy látni lehessen a munkaterhelések. Több virtuális Gépre kiterjedő konzisztencia engedélyezése hatással lehet a számítási feladat teljesítményére. Ajánlott használni, csak ha gép fut hello ugyanaz az alkalmazás és konzisztenciára van szükség.

    ![A replikáció engedélyezése](./media/site-recovery-physical-to-azure/policy.png)

14. Kattintson a **engedélyezze a replikálást**. Hello állapotának nyomon követheti **Védelemengedélyezési** feladat **beállítások** > **feladatok** > **Site Recovery-feladatok**. Hello után **Védelemvéglegesítési** feladat fut, hello gép készen áll a feladatátvételre.

Replikáció engedélyezése után hello mobilitási szolgáltatás telepítve van, ha ügyfélleküldéses telepítést. Miután hello mobilitási szolgáltatás leküldéses gépen telepítve, egy védelmi feladatot elindul, és sikertelen lesz. Hello meghibásodás után kell toomanually minden gép újraindításához. Majd hello védelmet feladat újra kezdődik, és a kezdeti replikálást.


### <a name="view-and-manage-azure-vm-properties"></a>Megtekintése és kezelése az Azure virtuális gép tulajdonságai

Azt javasoljuk, hogy ellenőrizze hello virtuális gép tulajdonságait, és hajtsa végre a szükséges módosításokat.

1. Kattintson a **replikált elemek**, és jelölje be hello gép. Hello **Essentials** panel gépek beállítások és állapotával kapcsolatos adatokat jeleníti meg.
2. A **tulajdonságok**, megtekintheti a replikációs és feladatátvételi adatait hello virtuális gép.
3. A **számítás és hálózat** > **számítási tulajdonságok**, megadhatja a hello Azure virtuális gép nevét és a cél méretét. Hello neve toocomply rendelkező módosítása [Azure-követelményeknek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) Ha kell.
4. Hello célhálózat alhálózatot és IP-cím rendelt toohello Azure virtuális gép beállításainak módosítása:

    a. Beállíthat hello cél IP-címet.

    b.  Ha nem ad meg egy címet, hello átvevő gép DHCP használja.

    c. Ha egy címet, amely nem használható feladatátadásra, feladatátvétel nem működik.

    d. cél IP-cím a feladatátvételi teszthez használható, ha hello címkészletében nem áll rendelkezésre a feladatátvételi teszt hálózatában hello hello.

    e. hálózati adapterek száma hello hello cél virtuális gép számára megadott hello mérete határozza meg:

     - Ha hello számú hálózati adaptert hello forrásgépen hello ugyanaz, mint a, vagy kevesebb, mint hello hello célként megadott gép méretéhez engedélyezett adapterek számával, akkor hello cél hello rendelkezik ugyanannyi adapter hello forrásaként.
     - Ha hello hello forrás virtuális gép adaptereinek száma meghaladja a hello cél méretéhez engedélyezett hello számát, majd hello cél maximális szolgál.
     - Például ha a forrásgépen két hálózati adaptert, és hello célgép mérete négy támogatja, hello célgépen két adapter rendelkezik. Ha hello forrásgépen két adapter, de hello támogatott célméretet támogatja a csak egy, a célgépen hello csak egy adapterrel rendelkezik.     
   - Ha hello virtuális gépnek több hálózati adaptert, az összes csatlakozó toohello ugyanazon a hálózaton.
   - Ha hello virtuális gépnek több hálózati adaptert, majd hello először egy hello listán látható lesz hello *alapértelmezett* hello Azure virtuális gép hálózati adapteréhez.
5. A **lemezek**, hello virtuális gép operációs rendszer és a replikált hello adatlemezek láthatja.

## <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása

Után minden kiépítését, futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik. Tekintse meg a gyors áttekintő videó elindítása előtt.

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]


1. egyetlen számítógépen, keresztül toofail a **beállítások** > **replikált elemek**, kattintson a **feladatátvételi teszt**.

    ![A feladatátvételi teszt](./media/site-recovery-vmware-to-azure/TestFailover.png)

2. a helyreállítás alatt toofail tervez, a **beállítások** > **helyreállítási tervek**, kattintson a jobb gombbal hello terv > **feladatátvételi teszt**. a helyreállítási terv toocreate [kövesse ezeket az utasításokat](site-recovery-create-recovery-plans.md).  
3. A **feladatátvételi teszt**, jelölje be hello Azure hálózati toowhich Azure virtuális gépek csatlakoznak a feladatátvételt követően.
4. Kattintson a **OK** toobegin hello feladatátvételi. Előrehaladásának hello VM tooopen a Tulajdonságok gombra kattintva vagy hello kattintva **feladatátvételi teszt** feladatot a tároló neve > **beállítások** > **feladatok**  >  **Site Recovery-feladatok**.
5. Hello feladatátvétel befejezése után meg kell jelennie toosee hello replika Azure machine hello Azure-portálon jelenik meg > **virtuális gépek**. Győződjön meg arról, hogy hello VM hello megfelelő méretét, csatlakoztatva van a megfelelő hálózati toohello, és fut-e.
6. Ha Ön [elvégezte a feladatátvételt követően](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover), meg kell tudni tooconnect toohello Azure virtuális Gépen.
7. Miután elkészült, kattintson **karbantartása a feladatátvételi teszt** a hello helyreállítási terv. A **megjegyzések**, és a feladatátvételi teszt hello kapcsolatos megfigyelések feljegyzéséhez mentéséhez. Ez a lépés törli a hello virtuális gépek feladatátvételi teszt során létrejött.

További információkért lásd: hello [teszt feladatátvételi tooAzure](site-recovery-test-failover-to-azure.md) dokumentum.

## <a name="next-steps"></a>Következő lépések

Miután beolvasni replikációs, és fut, nem tervezett kimaradás esetén a rendszer átadja a tooAzure, és Azure virtuális gépek replikálása hello adatokból jönnek létre. Amíg hátsó tooyour elsődleges hely sikertelen, a rendszer visszaadja toonormal műveletek munkaterheléseket és alkalmazásokat az Azure majd hozzáférni.

- [További](site-recovery-failover.md) feladatátvételek különböző típusú, és hogyan toorun őket.
- Ha a gépek replikálásához helyett és visszavétele, amelybe migrálna [további](site-recovery-migrate-to-azure.md#migrate-on-premises-vms-and-physical-servers).
- Ha fizikai gépeket replikál, akkor csak feladat-visszavétel tooa VMware-környezetben. [További információ a feladat-visszavétel](site-recovery-failback-azure-to-vmware.md).

## <a name="third-party-software-notices-and-information"></a>Harmadik felek szoftvereivel, megjegyzések és információk
Ne Localize vagy lefordítása

hello szoftver és a belső vezérlőprogram futó hello Microsoft termék vagy szolgáltatás alapul, vagy a szerződés magában foglalja a hello anyag projektek lenti (együttesen "külső kód"). A Microsoft nem eredeti szerzője hello hello külső kód. hello eredeti szerzői és a licenc, amely alatt a Microsoft külső kódok, kapott alábbi van beállítva.

hello leírtak A hello projektek-összetevőt az alább felsorolt külső kód kapcsolódik. Ilyen licencek és információkat csak tájékoztatási célokat szolgálnak. A külső kód folyamatban van a Microsoft szoftverlicenc-feltételek hello Microsoft termék vagy szolgáltatás a Microsoft által relicensed tooyou.  

b. hello információk kapcsolatos harmadik féltől származó kód összetevők végrehajtott elérhető tooyou Microsoft hello eredeti licencelési feltételei szerint.

hello teljes fájl található hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). A Microsoft kifejezetten át nem adott, az összes jogot fenntart által Ennek következménye, hogy belül, vagy más módon.
