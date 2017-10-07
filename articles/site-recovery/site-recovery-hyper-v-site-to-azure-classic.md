---
title: "a klasszikus portálon hello aaaReplicate Hyper-V virtuális gépek tooAzure |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooreplicate Hyper-V virtuális gépek-e tooAzure amikor gépek nem felügyel a VMM-felhőkben."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 3f4c4483-e3dd-495a-bd02-c16e9e28c88d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/21/2017
ms.author: raynew
ms.openlocfilehash: 12d08d950a79e956436cb03ffc87ab40e86c589e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-without-vmm-with-azure-site-recovery"></a>A helyszíni Hyper-V virtuális gépek és az Azure (VMM nélkül) az Azure Site Recovery közötti replikáció
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
> * [Klasszikus portál](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

Ez a cikk ismerteti, hogyan tooreplicate helyszíni Hyper-V virtuális gépek tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással, hello Azure-portálon. Ebben az esetben a Hyper-V kiszolgáló nem kezelt VMM-felhőkben.

A cikk elolvasása után bármely fűzhetnek hello lap alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="site-recovery-in-hello-azure-portal"></a>Site Recovery hello Azure-portálon

Az Azure két különböző [üzembe helyezési modellt](../resource-manager-deployment-model.md) kínál az erőforrások létrehozására és kezelésére: az Azure Resource Manager-modellt és a klasszikus modellt. Azure is rendelkezik a két portál – hello a klasszikus Azure portálon, és hello Azure-portálon.

Ez a cikk ismerteti, hogyan toodeploy hello a klasszikus portálon. klasszikus portál hello lehet használt toomaintain meglévő tárolóhoz. Nem hozható létre új tárolók hello klasszikus portál használatával.

## <a name="site-recovery-in-your-business"></a>A Site Recovery szerepe a vállalatban

A szervezetek kell, amely meghatározza, hogyan alkalmazások és adatok üzemben maradni a rendelkezésre álló tervezett és nem tervezett leállások során (BCDR) stratégiára, és toonormal működésre minél hamarabb helyreállítani. A Site Recovery a következőket kínálja:

* Külső helyszíni védelem a Hyper-V virtuális gépeken futó üzleti alkalmazások számára.
* Egy egyetlen helyen tooset, kezelheti és figyelheti a replikáció, feladatátvétel és helyreállítás.
* Egyszerű feladatátvétel tooAzure, és a feladat-visszavétel (visszaállítás) a helyszíni hely Azure tooHyper-V gazdagép kiszolgálókról.
* Több virtuális gépet tartalmazó helyreállítási tervek, így az egy szinthez tartozó alkalmazások számítási feladatai együtt hajtják végre a feladatátvételt.

## <a name="azure-prerequisites"></a>Azure-előfeltételek
* Szüksége van egy [Microsoft Azure](https://azure.microsoft.com/)-fiókra. Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.
* Szüksége van egy Azure storage fiók toostore replikált adatok. hello fiókot kell georeplikáció engedélyezve van. A hello és hello Azure Site Recovery-tárolónak ugyanabban a régióban, és társítani kell hello ugyanahhoz az előfizetéshez. [További tudnivalók az Azure storage](../storage/common/storage-introduction.md). Vegye figyelembe, hogy nem támogatjuk a mozgóátlag storage-fiókok hello használatával létrehozott [új Azure-portálon](../storage/common/storage-create-storage-account.md) erőforráscsoportok között.
* Szüksége lesz egy Azure virtuális hálózatra, hogy az Azure virtuális gépek csatlakoztatott tooa hálózati történő feladatátadást követően az elsődleges helyről.

## <a name="hyper-v-prerequisites"></a>Hyper-V előfeltételei
* Hello helyszíni forráshely szüksége lesz egy vagy több futtató **Windows Server 2012 R2** telepített hello Hyper-V szerepkörrel vagy **Microsoft Hyper-V Server 2012 R2**. Ez a kiszolgáló a következőket:
* Egy vagy több virtuális gépet tartalmaznia.
* Csatlakoztatott toohello Internet, lehet, közvetlen vagy proxyn keresztülmenő.
* Hello javítások Tudásbáziscikkben leírt futnia [2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977").

## <a name="virtual-machine-prerequisites"></a>A virtuális gép előfeltételei
Virtuális gépek tooprotect kívánt meg kell felelnie az [Azure virtuális gép követelményeinek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="provider-and-agent-prerequisites"></a>Provider és agent Előfeltételek
Az Azure Site Recovery üzembe helyezése részeként lesz hello Azure Site Recovery Provider telepítése, és hello Azure Recovery Services Agent minden Hyper-V kiszolgálón. Vegye figyelembe:

* Javasoljuk, hogy mindig a hello hello szolgáltató legújabb verziója és az ügynök futtatásához. Elérhetők hello Site Recovery portálon.
* Minden Hyper-V kiszolgáló az adott tárolóban kell rendelkeznie hello azonos hello szolgáltató verziói és az ügynök.
* hello hello kiszolgálón futó Provider csatlakozik tooSite helyreállítási keresztül hello internet. Ehhez a proxy nélkül hello hello Hyper-V kiszolgálón jelenleg konfigurált proxybeállításokat, vagy az Egyéni proxybeállítások szolgáltató telepítése közben konfigurálhatók. A kívánt toouse hello proxykiszolgáló férhessenek hozzá csatlakozni tooAzure hello URL-toomake lesz szüksége:

  * *.accesscontrol.windows.net
  * *.backup.windowsazure.com
  * *.hypervrecoverymanager.windowsazure.com
  * *.store.core.windows.net      
  * *.blob.core.windows.net
  - https://www.msftncsi.com/ncsi.txt
  - time.windows.com
  - time.nist.gov
* Továbbá a leírt hello IP-címek engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653) és a HTTPS (443) protokollt. Tooallow hello IP-címtartományok hello toouse és az USA nyugati régiója megtervezése Azure-régióban van.

Az ábrán látható hello különböző kommunikációs csatornák és a vezénylési és a replikálás a Site Recovery által használt portok

![B2A topológia](./media/site-recovery-hyper-v-site-to-azure-classic/b2a-topology.png)

## <a name="step-1-create-a-vault"></a>1. lépés: A tároló létrehozása
1. Jelentkezzen be toohello [kezelési portál](https://portal.azure.com).
2. Bontsa ki a **adatszolgáltatások** > **Recovery Services** kattintson **Site Recovery-tároló**.
3. Kattintson az **Új létrehozása** > **Gyorslétrehozás** elemre.
4. A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban.
5. A **régió**, válassza ki a hello hello tároló földrajzi régióját. támogatott toocheck régiók, tekintse meg a cikknek a földrajzi elérhetőséggel [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Kattintson a **Tároló létrehozása** elemre.

    ![Új tároló](./media/site-recovery-hyper-v-site-to-azure-classic/vault.png)

Ellenőrizze, hogy hello állapot sáv tooconfirm hello tároló sikeresen létrejött. hello tároló tulajdonosaként **aktív** a hello Recovery Services főoldalán.

## <a name="step-2-create-a-hyper-v-site"></a>2. lépés: Hyper-V hely létrehozása
1. Hello helyreállítási szolgáltatások oldalon kattintson hello tároló tooopen hello gyors kezdés lapon. Gyors üzembe helyezési hello ikon segítségével is megnyithatja.

    ![Első lépések](./media/site-recovery-hyper-v-site-to-azure-classic/quick-start-icon.png)
2. Hello legördülő listában válassza ki a **egy helyszíni Hyper-V hely és az Azure közötti**.

    ![Hyper-V hely forgatókönyv](./media/site-recovery-hyper-v-site-to-azure-classic/select-scenario.png)
3. A **hozzon létre egy Hyper-V hely** kattintson **hozzon létre Hyper-V hely**. Adjon meg egy helynevet, és mentse.

    ![Hyper-V-hely](./media/site-recovery-hyper-v-site-to-azure-classic/create-site.png)

## <a name="step-3-install-hello-provider-and-agent"></a>3. lépés: Hello szolgáltató és az ügynök telepítése
Hello szolgáltató és az ügynök telepítése minden tooprotect kívánt virtuális gépeket tartalmazó Hyper-V kiszolgálón.

Hyper-V fürt telepítése, elvégzi a 5-11 lépést hello feladatátvevő fürt mindegyik csomópontján. Miután regisztrált összes csomópontját, és engedélyezve van a védelem, virtuális gépek védi akkor is, ha az áttelepítés után hello fürt csomópontjai között.

1. A **készítse elő a Hyper-V kiszolgálók**, kattintson a **a regisztrációs kulcs letöltése** fájlt.
2. A hello **regisztrációs kulcs letöltése** kattintson **letöltése** következő toohello hely. Töltse le a hello kulcs tooa biztonságos helyen hello Hyper-V kiszolgáló által könnyen elérhető. Ezt követően 5 napig hello kulcs érvénytelen.

    ![Regisztrációs kulcs](./media/site-recovery-hyper-v-site-to-azure-classic/download-key.png)
3. Kattintson a **letöltési hello szolgáltató** tooobtain hello legújabb verziójára.
4. Hello fájl minden Hyper-V kiszolgálón futtatható kívánt tooregister hello tárolóban lévő állapottal. hello fájl két összetevőt telepíti:
   * **Az Azure Site Recovery Provider**– kezeli a kommunikációs és vezénylési közötti hello Hyper-V kiszolgáló és a hello Azure Site Recovery portálon.
   * **Az Azure Recovery Services Agent**– kezeli az adatokat átvitel hello forráskiszolgáló Hyper-V és az Azure storage futó virtuális gépek között.
5. A **Microsoft Update** lapon kérheti a frissítések telepítését. Ez a beállítás engedélyezve van, a Provider és Agent frissítéseket fogja telepíteni tooyour Microsoft Update-szabályzatnak megfelelően.

    ![Microsoft Update](./media/site-recovery-hyper-v-site-to-azure-classic/provider1.png)
6. A **telepítési** adja meg, ha azt szeretné, hogy a szolgáltató tooinstall hello és ügynököt a Hyper-V server hello.

    ![Telepítés helye](./media/site-recovery-hyper-v-site-to-azure-classic/provider2.png)
7. Telepítés befejezése után továbbra is tooregister hello server telepítő hello tárolóban lévő állapottal.
8. A hello **tároló beállításait** kattintson **Tallózás** tooselect hello kulcsot tartalmazó fájlt. Adja meg a hello Azure Site Recovery-előfizetést, hello tároló nevére, és hello Hyper-V hely toowhich hello Hyper-V kiszolgáló tartozik.

    ![Kiszolgáló regisztrációja](./media/site-recovery-hyper-v-site-to-azure-classic/provider8.PNG)
9. A hello **internetkapcsolat** lapra jut, hogy hello szolgáltató hogyan csatlakozzon a tooAzure Site Recovery. Válassza ki **alapértelmezett rendszer proxybeállításainak használata** toouse hello alapértelmezett internetkapcsolati beállításait hello kiszolgálón konfigurált. Ha nem ad meg egy értéket hello alapértelmezett beállításokat fogja használni.

   ![Internetbeállítások](./media/site-recovery-hyper-v-site-to-azure-classic/provider7.PNG)
10. Elindul a regisztráció tooregister hello server hello tárolóban lévő állapottal.

    ![Kiszolgáló regisztrációja](./media/site-recovery-hyper-v-site-to-azure-classic/provider15.PNG)
11. Regisztráció metaadatok befejezését követően a hello Hyper-V kiszolgáló Azure Site Recovery lekéri és hello kiszolgáló megjelenik a hello **Hyper-V helyek** hello lapján **kiszolgálók** lap hello tárolóban lévő állapottal.

### <a name="install-hello-provider-from-hello-command-line"></a>Telepítse a szolgáltató hello hello parancssorból
Másik megoldásként hello Azure Site Recovery Provider hello parancssorból is telepítheti. Ezt a módszert kell használnia, ha azt szeretné, hogy tooinstall hello szolgáltató Windows Server Core 2012 R2 rendszert futtató számítógépeken. Futtassa a parancssorból hello a következőképpen:

1. Töltse le a hello szolgáltató telepítési és a regisztrációs kulcs tooa mappát. A mappa lehet például a következő: C:\ASR.
2. A parancssor futtatása a rendszergazda, írja be:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Majd telepítse a szolgáltató hello futtatásával:

        C:\ASR> setupdr.exe /i
4. Futtassa a következő toocomplete regisztrációs hello:

        CD C:\Program Files\Microsoft Azure Site Recovery Provider
        C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>         

Ha paraméterek a következők:

* **/ Credentials**: Adja meg a letöltött hello regisztrációs kulcs hello helyét.
* **/ FriendlyName**: Adjon meg egy név tooidentify hello Hyper-V gazdagép-kiszolgálón. Ez a név fog megjelenni hello portál
* **/ EncryptionEnabled**: nem kötelező. Adja meg, hogy tooencrypt replika virtuális gépek Azure-ban (a inaktív adatok titkosítása).
* **/ proxyaddress**; **/proxyport**; **/proxyusername**; **/proxypassword**: nem kötelező. Adja meg a proxy paraméterek, ha azt szeretné, hogy toouse egyéni proxyt, vagy az aktuálisan használt proxy hitelesítést igényel.

## <a name="step-4-create-an-azure-storage-account"></a>4. lépés: Azure-tárfiók létrehozása
* A **erőforrás előkészítése** válasszon **Storage-fiók létrehozása** toocreate egy Azure storage-fiók, ha még nem rendelkezik ilyennel. hello fiókjának engedélyezett a georeplikáció kellene rendelkeznie. A hello és hello Azure Site Recovery-tárolónak ugyanabban a régióban, és társítani kell hello ugyanahhoz az előfizetéshez.

    ![Storage-fiók létrehozása](./media/site-recovery-hyper-v-site-to-azure-classic/create-resources.png)

> [!NOTE]
> 1. Nem támogatjuk a hello áthelyezés hello használatával létrehozott tárfiókok [új Azure-portálon](../storage/common/storage-create-storage-account.md) erőforráscsoportok között.
> 2. [A tárfiókok áttelepítési](../azure-resource-manager/resource-group-move-resources.md) erőforrás csoportok belül hello ugyanahhoz az előfizetéshez vagy előfizetések között nem támogatott a Site Recovery telepítéséhez használt tárfiókok.
>

## <a name="step-5-create-and-configure-protection-groups"></a>5. lépés: Csoportok létrehozása és konfigurálása védelme
Védelmi csoportok logikai csoportok használni kívánt virtuális gépek tooprotect használatával hello azonos védelmi konfigurációval. Védelmi beállítások tooa védelmi csoport alkalmaz, és ezek a beállítások alkalmazott tooall virtuális gépek toohello csoport hozzáadása.

1. A **létrehozása és a védelmi csoportok konfigurálása** kattintson **védelmi csoport létrehozása**. Ha bármely előfeltételek nem teljesülnek egy üzenet jelenik meg, és kattintson **részleteinek megtekintéséhez** további információt.
2. A hello **védelmi csoportok** lapon vegye fel egy védelmi csoportot. Adja meg egy nevet, a hello forráshely Hyper-V, a hello cél **Azure**, az Azure Site Recovery-előfizetés nevét, és hello Azure storage-fiók.

    ![Védelmi csoport](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group.png)
3. A **replikációs beállítások** set hello **másolás gyakorisága** toospecify milyen gyakran hello adatok különbözeti szinkronizálnia kell hello forrása és célja között. Beállíthat too30 másodperc, 5 perc és 15 perc.
4. A **helyreállítási pontok megőrzése** adja meg, hogy hány órán helyreállítási előzmények kell tárolni.
5. A **alkalmazáskonzisztens pillanatképek gyakorisága** megadhatja, hogy tootake pillanatképek, amely a kötet árnyékmásolata szolgáltatás (VSS) tooensure, amely az alkalmazások konzisztens állapotban legyenek, hello pillanatkép készítésének időpontjában. Alapértelmezés szerint ezek nincsenek használatban. Ellenőrizze, hogy az alapérték tooless mint hello további helyreállítási pontok száma. Ez csak akkor támogatott, ha hello virtuális gép Windows operációs rendszert futtat.
6. A **kezdeti replikáció kezdési ideje** adja meg, mikor hello védelmi csoportban lévő virtuális gépek kezdeti replikációja, legyenek elküldve az tooAzure.

    ![Védelmi csoport](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group2.png)

## <a name="step-6-enable-virtual-machine-protection"></a>6. lépés: Virtuális gép védelmének engedélyezése
Adja hozzá a virtuális gépek tooa védelmi csoport tooenable védelmének számukra.

> [!NOTE]
> A statikus IP-című linuxos virtuális gépek számára nem biztosítható védelem.
>
>

1. A hello **gépek** csoportosítja az üdvözlő védelmi csoportra, kattintson ** hozzáadása a virtuális gépek tooprotection lapon tooenable védelmi **.
2. A hello **a virtuális gép védelmének engedélyezése** lapon jelölje be hello virtuális gépeknek meg tooprotect.

    ![A virtuális gép védelmének engedélyezése](./media/site-recovery-hyper-v-site-to-azure-classic/add-vm.png)

    hello engedélyezése a védelmi feladatok kezdődik. Akkor tudja nyomon követni a hello **feladatok** fülre. Miután hello Védelemvéglegesítési feladatot futtat hello virtuális gép készen áll a feladatátvételre.
3. Védelmi után van beállítása, hogy a következőket teheti:

   * A virtuális gépek megtekintése **védett elemek** > **védelmi csoportok** > *protectiongroup_name*  >  **Virtuális gépek** hello toomachine részletes adatait részletezve **tulajdonságok** lapon...
   * A virtuális gépek feladatátvételi tulajdonságainak hello konfigurálása **védett elemek** > **védelmi csoportok** > *protectiongroup_name*  >  **Virtuális gépek** *virtual_machine_name* > **konfigurálása**. Konfigurálhatja:

     * **Név**: hello hello virtuális gép nevét, az Azure-ban.
     * **Méret**: hello célméretet hello virtuális gép, amely átadja a feladatokat.

       ![Virtuális gép tulajdonságainak konfigurálása](./media/site-recovery-hyper-v-site-to-azure-classic/vm-properties.png)
   * További virtuális gép beállítások konfigurálása *védett elemek** > **védelmi csoportok** > *protectiongroup_name*  >   **Virtuális gépek** *virtual_machine_name* > **konfigurálása**, többek között a következőket:

     * **A hálózati adapterek**: hello hálózati adapterek számát határozza meg hello mérete hello cél virtuális gép határozza meg. Ellenőrizze [virtuális gép mérete specifikációk](../virtual-machines/linux/sizes.md) hello hello virtuális gép mérete által támogatott hálózati adapterek száma.

       Módosítsa a virtuális gép hello méretét és hello beállítások mentéséhez, a hálózati adapterek száma hello változik-e, megnyitásakor **konfigurálása** lap hello legközelebb. a cél virtuális gépek hálózati adapterek száma hello nem minimális forrás virtuális gép hálózati adapterek hello számát és a hálózati adapterek hello kiválasztott hello virtuális gép mérete által támogatott maximális számát. Azt az alábbi leírásban is látható:

       * Ha hálózati adapterek hello forrásgépen hello száma kisebb vagy egyenlő, mint hello cél gép méretéhez engedélyezett adapterek toohello számát, majd hello cél lesz hello ugyanannyi adapter hello forrásaként.
       * Ha hello hello forrás virtuális gép adaptereinek száma meghaladja hello engedélyezett hello célméretet majd hello cél maximális fogja használni.
       * Ha például a forrásgépen két hálózati adapterrel és hello célgép mérete négy támogatja, hello célgépen két adapter fog működni. Ha hello forrásgépen két adapter, azonban a hello támogatott célméretet csak egy majd hello célgépen csak egy adapter fog működni.

     * **Azure-hálózat**: Adja meg a hello hálózati toowhich hello virtuális gép átadják a. Ha hello virtuális gépnek több hálózati adapter az összes adapter kell csatlakoztatott toohello azonos Azure-hálózatot.
     * **Alhálózati** minden hálózati adapter hello virtuális gépen, a feladatátvételt követően a select hello alhálózati hello Azure hálózati toowhich hello gépen kell csatlakozni.
     * **Cél IP-címe**: Ha a forrás virtuális gép hello hálózati adapter statikus konfigurált toouse egy IP-cím-e a ezután megadhatja a hello IP-cím a gép hello hello cél virtuális gép tooensure van hello azonos IP-címet a feladatátvételt követően .  Ha nem adja meg egy IP-cím majd bármely elérhető címek lesz hozzárendelve a feladatátvételi hello időpontjában. Ha megad egy címet, amely használatban van a feladatátvétel sikertelen lesz.

     > [!NOTE]
     > [Áttelepítési hálózatok](../azure-resource-manager/resource-group-move-resources.md) erőforrás csoportok belül ugyanahhoz az előfizetéshez hello vagy előfizetések között nem támogatott a Site Recovery üzembe helyezéséhez használt hálózatok.
     >

     ![Virtuális gép tulajdonságainak konfigurálása](./media/site-recovery-hyper-v-site-to-azure-classic/multiple-nic.png)




## <a name="step-7-create-a-recovery-plan"></a>7. lépés: A helyreállítási terv létrehozása
Rendelés tootest hello környezetben futtathatja egyetlen virtuális gép vagy egy vagy több virtuális gépet tartalmazó helyreállítási terv feladatátvételi tesztjét. [További](site-recovery-create-recovery-plans.md) : a helyreállítási terv létrehozása.

## <a name="step-8-test-hello-deployment"></a>8. lépés: Hello központi telepítés tesztelése
Nincsenek két módon toorun a teszt feladatátvételi tooAzure.

* **Azure-hálózat nélkül feladatátvételi teszt**– Ez a típus ellenőrzi, hogy hello virtuális megfelelően feláll az Azure-ban. hello virtuális gép nem lesz csatlakoztatott tooany Azure-hálózatot a feladatátvétel után.
* **Az Azure-hálózatot a feladatátvételi teszt**– az ilyen típusú feladatátvételi ellenőrzi, hogy a egész replikációs környezet hello megfelelően feláll-e, és toohello megadott cél Azure-hálózathoz csatlakozik, amely hello virtuális gépek a feladatátvételt. Vegye figyelembe, hogy a feladatátvételi teszthez hello alhálózati hello teszt virtuális gép lesz kell szerepelnek hello alhálózat hello replika virtuális gép alapján. Ez az különböző tooregular replikációs, ha egy replika virtuális gép hello alhálózati hello alhálózati hello forrás virtuális gép alapján.

Ha azt szeretné, toorun feladatátvételi teszt Azure-hálózat megadása nélkül nem kell tooprepare semmit.

a cél Azure-hálózat toocreate elkülönített új Azure-beli hálózat az Azure éles hálózattól (alapértelmezett működés, ha az Azure-ban hozzon létre egy új hálózatot) szüksége lesz egy feladatátvételi teszt toorun. Olvasási [feladatátvételi teszt futtatása](site-recovery-failover.md) további részleteket.

toofully a tooset hello infrastruktúra fel kell úgy, hogy hello replikált virtuális gép toowork elvárt replikáció- és hálózati telepítés tesztelésére. Egyik módja egy tartományvezérlőként, DNS-beli ennek során a virtuális gépek tootooset és azt használja a Site Recovery toocreate azt hello teszt feladatátvételi teszt futtatásával hálózati tooAzure.  [További](site-recovery-active-directory.md#test-failover-considerations) teszt feladatátvételi szempontokat részletező cikket az Active Directory kapcsolatban.

A feladatátvételi teszt hello futtassa a következőképpen:

> [!NOTE]
> tooget hello legjobb teljesítményt érhesse el egy feladatátvételt tooAzure, győződjön meg arról, hogy védett hello gépen hello Azure-ügynök telepítve. Az ügynök segít a rendszerindítás felgyorsításában és az esetlegesen felmerülő problémák diagnosztizálásában. A linuxos ügynök [itt](https://github.com/Azure/WALinuxAgent) található, a windowsos ügynök pedig [itt](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

1. A hello **helyreállítási tervek** lapra, válassza ki a hello tervet, és kattintson **feladatátvételi teszt**.
2. A hello **erősítse meg a feladatátvételi teszt** lapon jelölje be **nincs** vagy egy adott Azure-hálózatot.  Vegye figyelembe, hogy ha **nincs** hello a feladatátvételi teszt ellenőrzi, hogy hello a virtuális gép replikálása megfelelően tooAzure, de nem ellenőrzi a replikációs hálózat konfigurációját.

    ![A feladatátvételi teszt](./media/site-recovery-hyper-v-site-to-azure-classic/test-nonetwork.png)
3. A hello **feladatok** lapon nyomon követheti a feladatátvétel előrehaladását. Meg kell tudni toosee hello virtuális gép tesztelési replikájának hello Azure-portálon. Ha elkészült a beállítással tooaccess virtuális gépek a helyszíni hálózaton is kezdeményezhető a távoli asztali kapcsolat toohello virtuális gépek.
4. Amikor hello feladatátvételi eléri hello **teszt befejezése** fázist, kattintson a **teszt végrehajtása** toofinish hello feladatátvételi teszthez fel. Toohello részletezve **feladat** lapon tootrack feladatátvételi folyamat előrehaladásának és állapotának és tooperform szükséges műveleteket.
5. A feladatátvételt követően lesz képes toosee hello virtuális gép tesztelési replikájának hello Azure-portálon. Ha elkészült a beállítással tooaccess virtuális gépek a helyszíni hálózaton is kezdeményezhető a távoli asztali kapcsolat toohello virtuális gépek.

   1. Győződjön meg arról, hogy hello virtuális gép sikeresen elindul-e.
   2. Ha tooconnect toohello virtuális gépet az Azure-ban a távoli asztal hello feladatátvétel után, engedélyezze a távoli asztali kapcsolat hello virtuális gépen, hello a feladatátvételi teszt futtatása előtt. Konfigurálnia kell egy RDP-végpontot a virtuális gépen hello tooadd. Kihasználhatja a [Azure automation-runbook](site-recovery-runbook-automation.md) toodo, amely.
   3. Miután feladatátvételi a nyilvános IP cím tooconnect toohello virtuális gépek használatakor a távoli asztali kapcsolat segítségével győződjön meg arról nincs érvényben tartományi szabályzatok, amelyek meggátolják tooa virtuális gép nyilvános cím segítségével csatlakozzon.
6. Hello a tesztelés után hajtsa végre a következő hello:

   * Kattintson a **hello a feladatátvételi teszt befejeződött**. Hello karbantartása környezet tooautomatically power tesztelése ki, és törölje a hello teszt virtuális gépek.
   * Kattintson a **megjegyzések** toorecord hello feladatátvételi teszthez kapcsolatos megfigyelések feljegyzéséhez mentse.
7. Amikor hello feladatátvételi eléri hello **teszt befejezése** fázis Befejezés hello ellenőrzése az alábbiak szerint:
   1. Nézet hello replika virtuális gép hello Azure-portálon. Győződjön meg arról, hogy hello a virtuális gép sikeresen elindul.
   2. Ha elkészült a beállítással tooaccess virtuális gépek a helyszíni hálózaton is kezdeményezhető a távoli asztali kapcsolat toohello virtuális gépek.
   3. Kattintson a **teljes hello teszt** toofinish azt.
   4. Kattintson a **megjegyzések** toorecord hello feladatátvételi teszthez kapcsolatos megfigyelések feljegyzéséhez mentse.
   5. Kattintson a **hello a feladatátvételi teszt befejeződött**. Hello karbantartása környezet tooautomatically power tesztelése ki, és hello teszt virtuális gép törlése.

## <a name="next-steps"></a>Következő lépések
Ha sikerült beállítania és elindítani az üzemelő példányt, [ismerkedjen meg részletesebben](site-recovery-failover.md) a feladatátvételi folyamattal.
