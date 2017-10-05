---
title: "A VMM-ben a Hyper-V virtuális gépek replikálása másodlagos helyre (a klasszikus Azure portál) |} Microsoft Docs"
description: "Ez a cikk ismerteti a Hyper-V virtuális gépek VMM-felhőkben replikálása az Azure Site Recovery VMM másodlagos helyre."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: a63acba3-8fb3-4926-aa29-b1e64a765681
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: raynew
ms.openlocfilehash: 6814f62f73bad272229205f4768dcce5947117d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a>Hyper-V virtuális gépek replikálása másodlagos VMM-hely VMM-felhőkben
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Klasszikus portál](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Az Azure Site Recovery szolgáltatás a virtuális gépek és a fizikai kiszolgálók replikálásával, feladatátvételével és helyreállításával segít a vállalatoknak az üzletmenet-folytonossági és vészhelyreállítási (BCDR) stratégia megvalósításában. A gépeket az Azure-ba, vagy egy másodlagos helyszíni adatközpontba replikálhatja. A szolgáltatás gyors áttekintéséhez olvassa el az [Azure Site Recovery szolgáltatás mibenlétével foglalkozó](site-recovery-overview.md) cikket.

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti a Hyper-V gazdakiszolgálókra VMM másodlagos hely az Azure Site Recovery segítségével a VMM-felhőkben felügyelt Hyper-V virtuális gépek replikálása.

A cikk Előfeltételek tartalmaz, akkor azt ismerteti, hogyan állítsa be a Site Recovery-tároló, telepítse az Azure Site Recovery Provider a forrás és cél VMM-kiszolgálók, regisztrálja a kiszolgálót a tárolóban, adja meg a VMM-felhő védelmi beállításait és majd engedélyezni a védelmet a Hyper-V virtuális gépek. Végül megismerheti a feladatátvételi teszt végrehajtásának módját, amellyel ellenőrizheti, hogy minden az elvárások szerint működik-e.

Megjegyzéseit vagy kérdéseit a cikk alján, vagy az [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) teheti fel.

## <a name="architecture"></a>Architektúra
A következő ábrán látható, a különböző kommunikációs csatornák és a vezénylési és a replikálás az Azure Site Recovery által használt portok

![E2E topológia](./media/site-recovery-vmm-to-vmm-classic/e2e-topology.png)

## <a name="before-you-start"></a>Előkészületek
Győződjön meg arról, hogy az előfeltételek teljesülnek:

| **Előfeltételek** | **Részletek** |
| --- | --- |
| **Azure** |Szüksége van egy [Microsoft Azure](https://azure.microsoft.com/)-fiókra. Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is. [További információk](https://azure.microsoft.com/pricing/details/site-recovery/) a Site Recovery díjszabásáról. |
| **VMM** |Legalább egy VMM-kiszolgáló van szüksége.<br/><br/>A VMM-kiszolgáló legalább futnia kell a System Center 2012 SP1 a legújabb kumulatív frissítéseket.<br/><br/>Ha szeretne egy egyetlen VMM-kiszolgálóval védelem beállítása, konfigurálva a kiszolgálón legalább két felhők kell.<br/><br/>Ha azt szeretné, védelem és a két VMM-kiszolgáló központi telepítése, minden kiszolgáló rendelkeznie kell legalább egy felhőben konfigurálva a védeni kívánt elsődleges VMM-kiszolgálón, és a másodlagos VMM-kiszolgáló védelmi és helyreállítási használni kívánt egy felhőnek<br/><br/>Az összes VMM-felhő beállítása a Hyper-V-kapacitásprofilt kell rendelkeznie.<br/><br/>A védelemmel ellátni kívánt forrásfelhőnek legalább egy VMM-gazdagépcsoportot kell tartalmaznia. |
| **Hyper-V** |Az elsődleges és másodlagos VMM-gazdagépcsoportot lévő egy vagy több Hyper-V gazdakiszolgálók, és minden Hyper-V gazdakiszolgálón futó virtuális gépek egy vagy több szükséges.<br/><br/>A gazdagép és a cél Hyper-V kiszolgálók legalább futnia kell a Hyper-V szerepkör és a Windows Server 2012 és a legújabb frissítések telepítve van.<br/><br/>A védelemmel ellátni kívánt virtuális gépeket tartalmazó Hyper-V-kiszolgálóknak a VMM-felhőben kell futniuk.<br/><br/>Ha a Hyper-V egy fürtben fut, vegye figyelembe, hogy a fürtszervező nem hozza létre automatikusan Ha egy statikus IP cím alapú fürtöt. A fürtszervező kézzel konfigurálásához szükséges. [További információkat](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) Aidan Finn blogbejegyzésében talál. |
| **Hálózatleképezés** |Győződjön meg arról, hogy a replikált virtuális gépek optimális kerülnek, a másodlagos Hyper-V gazdakiszolgálókra feladatátvétel után, valamint hogy megfelelő Virtuálisgép-hálózatok csatlakozhatnak-e a hálózatra való leképezést is konfigurálhat. Ha nem adja meg a hálózatleképezés, a replika virtuális gépek nem csatlakozik semmilyen hálózathoz feladatátvételt követően.<br/><br/>Állítsa be a hálózatleképezés üzembe helyezése során, ellenőrizze, hogy a forrás Hyper-V gazdakiszolgálón futó virtuális gépek csatlakoznak-e a VMM Virtuálisgép-hálózatot. Erre a hálózatra kapcsolódnia kell egy logikai hálózatot, amely társítva van a felhőben. < br /<br/>A megcélzott felhőt, a helyreállításhoz használható másodlagos VMM kiszolgálón kell lennie a megfelelő Virtuálisgép-hálózatot, és azt pedig kösse össze egy megfelelő logikai hálózatot, amely társítva van a megcélzott felhőt. |
| **Tárolási leképezése** |Alapértelmezés szerint a cél Hyper-V gazdakiszolgálón replikálja a forrás Hyper-V gazdakiszolgálón futó virtuális gép replikált adatok tárolja az alapértelmezett hely a cél Hyper-V gazdagép, a Hyper-V kezelőjében feltüntetett. Pontosabban a replikált adatok tárolására konfigurálhatja a tárolási leképezése<br/><br/> Tároló leképezési konfigurálásához szüksége tárhelybesorolások a forrás és cél VMM-kiszolgálók központi telepítésének megkezdése előtt. |

## <a name="step-1-create-a-site-recovery-vault"></a>1. lépés: Site Recovery-tároló létrehozása
1. Jelentkezzen be a regisztrálni kívánt VMM-kiszolgálón a [felügyeleti portálra](https://portal.azure.com).
2. Bontsa ki a **adatszolgáltatások** > **Recovery Services** kattintson **Site Recovery-tároló**.
3. Kattintson az **Új létrehozása** > **Gyorslétrehozás** elemre.
4. A **Név** mezőben adjon meg egy, a tárolót azonosító rövid nevet.
5. A **régió** válassza ki a tároló földrajzi területét. A támogatott régiók megtekintéséhez olvassa el az [Azure Site Recovery – Díjszabás](http://go.microsoft.com/fwlink/?LinkId=389880) című cikknek a földrajzi elérhetőséggel foglalkozó részét.
6. Kattintson a **Tároló létrehozása** elemre.

    ![tároló létrehozása](./media/site-recovery-vmm-to-vmm-classic/create-vault.png)

Az állapotsor ellenőrizze, hogy létrejött-e a tárolóban. Ezt követően a tároló **Aktív** állapottal jelenik meg a Recovery Services főoldalán.

## <a name="step-2-generate-a-vault-registration-key"></a>2. lépés: A tároló regisztrációs kulcsának létrehozása
Hozza létre a tároló regisztrációs kulcsát. Az Azure Site Recovery Provider letöltését, valamint VMM-kiszolgálóra történő telepítését követően ezzel a kulccsal regisztrálhatja a VMM-kiszolgálót a tárolóban.

1. A **Recovery Services** lapon kattintson a kívánt tárolóra az Első lépések lap megnyitásához. Az Első lépések bármikor megnyitható az ikon segítségével is.

    ![Első lépések ikon](./media/site-recovery-vmm-to-vmm-classic/quick-start-icon.png)
2. A legördülő listában válassza ki a **két helyszíni VMM-helyek közötti**.
3. A **előkészítése a VMM-kiszolgálók**, kattintson a **Generate regisztrációs kulcsot tartalmazó fájlt**. A rendszer létrehozza a kulcsfájlt, amely ezt követően 5 napig érvényes. Ha a VMM-kiszolgálóról nem készül hozzáférni az Azure-portálon kell ezt a fájlt másolja a kiszolgálóra.

    ![Regisztrációs kulcs](./media/site-recovery-vmm-to-vmm-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>3. lépés: Az Azure Site Recovery Provider telepítése
1. Az a **gyors üzembe helyezés** lap **előkészítése a VMM-kiszolgálók**, kattintson a **töltse le a Microsoft Azure Site Recovery Provider telepítése a VMM Server** a legújabb verzió beszerzéséhez a Provider telepítőfájlját.
2. Futtassa ezt a fájlt a forrás VMM-kiszolgálón.

   > [!NOTE]
   > Ha a VMM fürtben üzemel, és Ön először telepíti a Providert, telepítse azt aktív csomópontra, majd fejezze be a telepítést, és regisztrálja a VMM-kiszolgálót a tárolóban. Ezt követően telepítse a Providert a többi csomópontra. Ne feledje, hogy a szolgáltató frissítés frissítése az összes olyan csomóponton, mert azok kell futtatniuk a szolgáltató azonos verziója.
   >
   >
3. A telepítő does néhány **Előfeltételek ellenőrzése** és a VMM szolgáltatást, a Provider telepítésének elindítására engedélyt kér. A rendszer a telepítés befejezését követően automatikusan újraindítja a VMM szolgáltatást. Ha VMM-fürtben végzi a telepítést, a telepítő megkéri a fürtszerepkör leállítására.
4. A **Microsoft Update** lapon kérheti a frissítések telepítését. Ha bekapcsolja ezt a funkciót, a rendszer a Microsoft Update-szabályzatnak megfelelően telepíteni fogja a Provider frissítéseit.

    ![Microsoft Update](./media/site-recovery-vmm-to-vmm-classic/ms-update.png)
5. A telepítés helyének beállítása  **<SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin**. Kattintson a telepítés gombra a Provider telepítésének megkezdése.

    ![Telepítési hely](./media/site-recovery-vmm-to-vmm-classic/install-location.png)
6. A Provider telepítését követően a kiszolgálónak a tárolóban való regisztrálásához kattintson a **Register** (Regisztrálás) elemre.

    ![Telepítés kész](./media/site-recovery-vmm-to-vmm-classic/install-complete.png)
7. A **Tároló neve** résznél ellenőrizze a tároló nevét, amelyben a kiszolgálót regisztrálni fogja. Kattintson a *Tovább* gombra.

    ![Kiszolgáló regisztrációja](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)
8. Az **Internet Connection** (Internetkapcsolat) lapon adja meg, hogy a VMM-kiszolgálón futó Provider hogyan csatlakozzon az internethez. Válassza a **Connect with existing proxy settings** (Csatlakozás a meglévő proxybeállításokkal) lehetőséget, ha a kiszolgálón konfigurált alapértelmezett internetkapcsolat-beállításokat szeretné használni.

    ![Internetbeállítások](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

   * Ha egyéni proxyt szeretne használni, állítsa be még a Provider telepítése előtt. Miután megadta az egyéni proxy beállításait, a rendszer egy teszt lefuttatásával ellenőrzi a proxykapcsolatot.
   * Ha egyéni proxyt használ, vagy az alapértelmezett proxy meg kell adnia a proxy adatait, beleértve a proxy címét és portját hitelesítést igényel.
   * A következő URL-címeknek elérhetőnek kell lenniük a VMM-kiszolgálóról és a Hyper-V gazdagépekről:
     * *.hypervrecoverymanager.windowsazure.com
     * *.accesscontrol.windows.net
     * *.backup.windowsazure.com
     * *.blob.core.windows.net
     * *.store.core.windows.net
   * Engedélyezze az [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (Azure Datacenter IP-tartományai) részben említett IP-címeket, valamint a HTTPS (443) protokollt. Egy engedélyezési listára kell tennie a használni kívánt Azure-régió és az USA nyugati régiójának IP-tartományait.
   * Ha egyéni proxyt használ, a rendszer automatikusan létrehoz egy, a megadott hitelesítő adatokat alkalmazó VMM RunAs-fiókot (DRAProxyAccount). Állítsa be úgy a proxykiszolgálót, hogy ez a fiók elvégezhesse a hitelesítést. A VMM RunAs-fiók beállításait a VMM-konzolban módosíthatja. Ehhez a **Beállítások** munkaterületen bontsa ki a **Biztonság** lehetőséget, kattintson a **Futtató fiókok** elemre, majd módosítsa a DRAProxyAccount jelszavát. A beállítás akkor lép érvénybe, ha újraindítja a VMM szolgáltatást.
9. A **Regisztrációs kulcs** résznél válassza ki az Azure Site Recoveryből letöltött, majd a VMM-kiszolgálóra másolt kulcsot.
10. A titkosítási beállítást csak akkor használja a rendszer, amikor a VMM-felhőkben található Hyper-V virtuális gépeket az Azure-ba replikálja. Másodlagos helyre történő replikáláskor a beállítást nem használja a rendszer.
11. A **Kiszolgáló neve** mezőben adjon meg egy, a tárolóban regisztrált VMM-kiszolgálót azonosító rövid nevet. Fürtkonfiguráció használata esetén adja meg a VMM-fürtszerepkör nevét.
12. A **Synchronize cloud metadata** (Felhőmetaadatok szinkronizálása) mezőben válassza ki, hogy szeretné-e a VMM-kiszolgáló összes felhőjének metaadatait szinkronizálni a tárolóval. Ezt a műveletet kiszolgálónként csak egyszer szükséges elvégezni. Ha nem szeretné az összes felhőt szinkronizálni, ne jelölje be a jelölőnégyzetet, és szinkronizálja egyenként a felhőket a VMM-konzolban, a felhők tulajdonságainál.
13. A folyamat befejezéséhez kattintson a **Next** (Tovább) gombra. A regisztrációt követően az Azure Site Recovery lekéri a metaadatokat VMM-kiszolgálóról. A kiszolgáló megjelenik **VMM-kiszolgálókon** > **kiszolgálók** a tárolóban lévő állapottal.

    ![Kiszolgálók](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

### <a name="command-line-installation"></a>Parancssori telepítés
Az Azure Site Recovery Provider a parancssorból is telepíthető. Ez a módszer segítségével telepítse a szolgáltatót a egy Server CORE a Windows Server 2012 R2.

1. Töltse le egy mappába a Provider telepítőfájlját és a regisztrációs kulcsot. A mappa lehet például a következő: C:\ASR.
2. A System Center Virtual Machine Manager szolgáltatás leállítása
3. Ezek a parancsok futtatásával parancssort a Provider telepítőjének kibontásához **rendszergazda** jogosultságokkal:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Telepítse a szolgáltató futtatásával:

        C:\ASR> setupdr.exe /i
5. Regisztrálja a szolgáltatót futtatásával:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Ha a paraméterek a következők:

* **/ Credentials**: kötelező paraméter, amely megadja a helyet, ahol a regisztrációs kulcs fájlját található  
* **/FriendlyName**: kötelező paraméter, amely a Hyper-V gazdakiszolgáló nevét tartalmazza úgy, ahogy az Azure Site Recovery portálon megjelenik.
* **/ EncryptionEnabled**: nem kötelező paraméter, amely kell használnia csak a Azure forgatókönyv a VMM-ben, ha a virtuális gépek Azure-ban aktívan titkosítási van szüksége. Győződjön meg arról, hogy rendelkezik-e a fájl nevét a **.pfx** bővítmény.
* **/proxyAddress**: nem kötelező paraméter, amely a proxykiszolgáló címét határozza meg.
* **/proxyport**: nem kötelező paraméter, amely a proxykiszolgáló portját határozza meg.
* **/ proxyusername**: nem kötelező paraméter, amely a Proxy felhasználónevét határozza meg (Ha a proxy hitelesítést igényel).
* **/ proxypassword**: nem kötelező paraméter, amely a hitelesítéshez a proxykiszolgáló (Ha a proxy hitelesítést igényel) jelszavát adja meg.  

## <a name="step-4-configure-cloud-protection-settings"></a>4. lépés: Konfigurálja a felhő védelmi beállításait
Miután a VMM-kiszolgáló regisztrálva van, konfigurálhatja a felhő védelmi beállításait. Ha engedélyezte a **Synchronize cloud data a tárolóval** telepítésekor a szolgáltató, a VMM-kiszolgálón futó összes felhő fog megjelenni a **védett elemek** lapon a tárolóban lévő állapottal. Ha Ön nem képes szinkronizálni az adott felhő az Azure Site Recovery a **általános** lapján a VMM-konzolon a felhő tulajdonságlapján.

![Közzétett felhő](./media/site-recovery-vmm-to-vmm-classic/clouds-list.png)

1. A Quick Start (Első lépések) lapon kattintson a **Set up protection for VMM clouds** (VMM-felhők védelmének beállítása) elemre.
2. Az a **VMM-Felhőkben** lapra, válassza ki a felhőt, amelyet szeretne konfigurálni, és navigáljon a **konfigurációs** fülre.
3. A **cél**, jelölje be **VMM**.
4. A **célhelye**, válassza ki a helyszíni VMM-kiszolgálót, amely kezeli a helyreállításhoz használni kívánt felhőt.
5. A **céloz felhő**, válassza ki a megcélzott felhőt, a virtuális gépek a forrás felhőben feladatátvételre használni kívánt. Vegye figyelembe:

   * Azt javasoljuk, hogy bejelöli-e, amely megfelel a virtuális gépek fogja védeni a helyreállítási cél felhő.
   * Felhő csak egyetlen felhőpárnál is tartozhat – egy elsődleges vagy a megcélzott felhőt.
6. A **másolás gyakorisága**, adja meg, milyen gyakran szinkronizálja az adatokat a 5he forrása és célja között. Vegye figyelembe, hogy ez a beállítás csak megfelelő a Hyper-V gazdagépen futó Windows Server 2012 R2 esetén. A többi kiszolgáló öt perc az alapértelmezett beállítás használatos.
7. A **további helyreállítási pontok**, adja meg, hogy további pontok létrehozásához. Az alapértelmezett nulla érték azt jelzi, hogy csak a legutóbbi helyreállítási pontot az elsődleges virtuális gépen tárolt replika gazdakiszolgálón. Vegye figyelembe, hogy több helyreállítási pont engedélyezése igényel további tárhely minden helyreállítási ponton tárolt pillanatképek. Alapértelmezés szerint helyreállítási pontokat óránként létrehozott, így az egyes helyreállítási pontok tartalmaz egy órányi adat tekinthető meg. A helyreállítási pont a VMM-konzolban a virtuális géphez hozzárendelt értéke nem lehet kisebb, mint az Azure Site Recovery konzolján hozzárendelt értéke.
8. A **alkalmazáskonzisztens pillanatképek gyakorisága**, adja meg, hogyan gyakran alkalmazáskonzisztens pillanatképek létrehozásához. A Hyper-V két különböző pillanatképtípust használ: az egyik a standard pillanatkép, amely a virtuális gép egészét lefedő növekményes pillanatképet, a másik pedig az alkalmazáskonzisztens pillanatkép, amely a virtuális gépen futó alkalmazások adatairól készült, időponthoz kötött pillanatképet jelent. Az alkalmazáskonzisztens pillanatképek a kötet árnyékmásolása szolgáltatás (VSS) segítségével garantálják, hogy az alkalmazások konzisztens állapotban legyenek a pillanatkép készítésének időpontjában. Ne feledje, hogy az alkalmazáskonzisztens pillanatképek bekapcsolása negatív hatással lesz a forrás virtuális gépeken futó alkalmazások teljesítményére. Ügyeljen rá, hogy az itt megadott érték kisebb legyen a további beállított helyreállítási pontok számánál.

    ![Védelmi beállítások konfigurálása](./media/site-recovery-vmm-to-vmm-classic/cloud-settings.png)
9. A **adatátvitel tömörítésének**, adja meg, hogy van-e a replikált adatok tömörített.
10. A **hitelesítési**, adja meg, hogy az elsődleges és a helyreállítási Hyper-V gazdakiszolgálók közötti forgalom hitelesítése. Válassza ki a HTTPS, hacsak nincs konfigurálva a Kerberos-környezet működőképes. Az Azure Site Recovery automatikusan HTTPS-hitelesítési tanúsítványok konfigurálja. Manuális beállításokra nincs is szükség. Kerberos csak akkor válassza, ha a Kerberos jegyet a gazdakiszolgálók a kölcsönös hitelesítéshez használható. Alapértelmezés szerint a Windows tűzfalat a Hyper-V gazdakiszolgálók port 8083 és 8084 (a tanúsítványok) fognak megnyílni. Vegye figyelembe, hogy ez a beállítás csak a Windows Server 2012 R2 rendszeren futó Hyper-V gazdakiszolgálók szükséges.
11. A **Port**, módosítsa a port számát, amelyen a forrás- és a cél számítógépek a replikációs forgalom figyelésére. Például a beállítás lehet, hogy módosításához, ha azt szeretné alkalmazni a szolgáltatásminőség (QoS) a hálózati sávszélesség-szabályozási a replikációs forgalmat. Ellenőrizze, hogy a portot más alkalmazás nem használja, és hogy meg nyitva a tűzfal beállításait.
12. A **replikációs mód**, adja meg a kezdeti replikálás célhelyeket forrásból származó adatok kezelésének, rendszeres replikáció megkezdése előtt:

    * **Hálózaton keresztül**– időigényes és erőforrás-igényes lehet adatok másolása a hálózaton keresztül. Azt javasoljuk, hogy ezt a beállítást használja, ha a felhő viszonylag kis virtuális merevlemezzel rendelkező virtuális gépeket tartalmaz, és az elsődleges helyhez csatlakozik-e a másodlagos hely nagy sávszélességű kapcsolaton keresztül. Megadhatja, hogy a másolat kell azonnal elindítani, vagy válassza ki a megfelelő. Ha hálózati replikáció használata esetén ajánlott ütemezni a csúcsidőn.
    * **Kapcsolat nélküli**– Ez a módszer határozza meg, hogy a kezdeti replikálás történik-e külső adathordozó segítségével. Ez akkor hasznos, ha szeretné elkerülni a hálózati teljesítményt, vagy földrajzilag távoli helyekre teljesítménycsökkenést. Ezt a módszert meg kell adnia az Exportálás helyére a forrás-felhő, és a célfelhő az importálási helyre. Ha engedélyezi a virtuális gép védelmét, a rendszer átmásolja a virtuális merevlemez megadott exportálási helyét. Elküldi a célhelyhez, és az importálás helyre másolhatja. A rendszer átmásolja a replika virtuális gépeket az importált adatok.
13. Válassza ki **replika virtuális gép törlése** adhatja meg, hogy a replika virtuális gép törölhetők, ha kikapcsolja a virtuális gép védelmét; ehhez válassza a **törölje a virtuális gépvédelmet**lehetőséget a virtuális gépek a felhők tulajdonságainál. Ezzel a beállítással, védelmének letiltásakor a virtuális gép Azure Site Recovery törlődik, a Site Recovery beállítások a virtuális gép eltávolítása a VMM-konzolon, és a replika törlése.

    ![Védelmi beállítások konfigurálása](./media/site-recovery-vmm-to-vmm-classic/cloud-settings-replica.png)

A beállítások mentését követően a rendszer létrehoz egy feladatot, amelyet aztán a **Jobs** (Feladatok) lapon figyelhet. Ezzel a VMM-forrásfelhőben található összes Hyper-V gazdakiszolgáló készen áll a replikációra. Felhőbeállítások módosíthatók a **konfigurálása** fülre. Ha azt szeretné, hogy a hely vagy a cél célfelhő módosításához távolítsa el a felhő konfigurációját, és az konfigurálja újra a felhőben.

### <a name="prepare-for-offline-initial-replication"></a>Offline kezdeti replikálás előkészítése
A következő műveleteket hajthatja végre készíti elő a kezdeti replikálás offline lesz szüksége:

* A forráskiszolgálón lesz adjon meg egy elérési útja, amelyből az adatok exportálása akkor kerül sor. Rendelje hozzá a teljes hozzáférés NTFS és a megosztási engedélyek az Exportálás elérési úton a VMM szolgáltatást. A cél kiszolgálón lesz adjon meg egy elérési útja, amelyből az adatok importálását történik. Rendelje hozzá az importálási útvonalat ugyanazokkal az engedélyekkel.
* Ha az importálási vagy exportálási elérési út megosztása, rendelje hozzá a távoli számítógépen, amelyen a megosztott megtalálható a VMM szolgáltatás fiókja rendszergazda, a kiemelt felhasználó, a Nyomtatófelelősök vagy a kiszolgáló operátor csoporttagság.
* Ha a futtató fiókok segítségével vegyen fel a gazdagépet, az importálási és exportálási útvonalakra, rendelje hozzá az olvasási és írási engedéllyel a futtató fiókokat a VMM-ben.
* Az importálási és exportálási megosztások nem kell található egy Hyper-V gazdagép-kiszolgálón, amely minden olyan számítógépen, mert visszacsatolási konfiguráció nem támogatott a Hyper-v.
* Az Active Directory minden Hyper-v gazdagép-kiszolgálón szeretné védeni, akkor engedélyezze és konfigurálja a virtuális gépeket tartalmazó által korlátozott delegálás hogy bízzon meg a távoli számítógépeken, amelyeken az importálási és exportálási útvonalak találhatók, az alábbiak szerint:
  1. A tartományvezérlőn nyissa meg a **Active Directory – felhasználók és számítógépek**.
  2. A konzolfán kattintson **tartománynév** > **számítógépek**.
  3. Kattintson a jobb gombbal a Hyper-V gazdagép-kiszolgálónév > **tulajdonságok**.
  4. Az a **delegálás** lapon kattintson a T**ezen a számítógépen csak a megadott szolgáltatások delegálhatók Rozsdás**.
  5. Kattintson a **bármely hitelesítési protokoll**.
  6. Kattintson a **hozzáadása** > **felhasználók és számítógépek**.
  7. Írja be a nevét, a számítógép, amelyen az Exportálás elérési útja > **OK**. Választható szolgáltatások közül, tartsa lenyomva a CTRL billentyűt, és kattintson a **cifs** > **OK**. Ismételje meg a számítógép, amelyen az importálási útvonalat nevét. Ismételje meg a Hyper-V-gazdagép további kiszolgálókat a megfelelő.

## <a name="step-5-configure-network-mapping"></a>5. lépés: Hálózatleképezés konfigurálása
1. A Quick Start (Első lépések) lapon kattintson a **Map networks** (Hálózatok leképezése) elemre.
2. Válassza ki a forrás VMM-kiszolgálón, amelyről hálózatok leképezni kívánt, és a cél VMM-kiszolgáló, amelyhez a rendszer a hálózatok rendeli. A forrás hálózatok listáját és azok kapcsolódó célhálózatok jelennek meg. Jelenleg nincsenek leképezve hálózatok jelenik meg egy üres érték.
3. Válasszon egy hálózatot a **hálózati forrás** > **térkép**. A szolgáltatás észleli a Virtuálisgép-hálózatokat a célkiszolgálón, és megjeleníti őket. Kattintson a forrás és cél hálózati nevek mindegyik alhálózatok megtekintése mellett információs ikonra.

    ![Hálózatleképezés konfigurálása](./media/site-recovery-vmm-to-vmm-classic/network-mapping1.png)
4. A párbeszédpanelen válassza ki azt a Virtuálisgép-hálózatok a cél VMM-kiszolgálóval.

    ![Válasszon egy célhálózatot](./media/site-recovery-vmm-to-vmm-classic/network-mapping2.png)
5. Amikor kiválaszt egy célhálózatot, a védett felhő, amelyek a forrás hálózati jelennek meg. A védelemhez használt felhőkhöz van társítva az elérhető célkiszolgálók hálózatokhoz is látható. Azt javasoljuk, hogy bejelöli-e a cél hálózati védelmet alkalmaz minden felhő számára elérhető. Vagy is a VMM-kiszolgálóról, és adja hozzá a logikai hálózatot, válassza a kívánt virtuálisgép-hálózatnak megfelelő a felhő tulajdonságainak módosítása.
6. A leképezési művelet végrehajtásához kattintson a pipára. A feladatok elindításának nyomon követéséhez a leképezési művelet előrehaladását. Tekintheti meg a **feladatok** fülre.

## <a name="step-6-configure-storage-mapping"></a>6. lépés: Konfigurálja a tárolási leképezése
Alapértelmezés szerint a cél Hyper-V gazdakiszolgálón replikálja a forrás Hyper-V gazdakiszolgálón futó virtuális gép replikált adatok tárolja az alapértelmezett hely a cél Hyper-V gazdagép, a Hyper-V kezelőjében feltüntetett. Pontosabban a replikált adatok tárolására az alábbiak szerint konfigurálhatja tárolási leképezéseket:

1. Tárhelybesorolások határozza meg a forrás- és a célként megadott VMM-kiszolgálókon. [További információk](https://technet.microsoft.com/library/gg610685.aspx). Besorolások forrás és cél Hyper-V gazdagép kiszolgálója számára elérhetőnek kell lennie. Besorolások tárolási ugyanolyan nem szükséges. Például a forrás besorolást, amely tartalmazza a cél a besoroláshoz, amely tartalmazza a megosztott fürtkötetek SMB-megosztások is leképezheti.
2. Miután besorolások pedig a következők teljesülnek leképezéseket hozhat létre. Ehhez a a **gyors üzembe helyezés** lap > **tárolási leképezése**.
3. Kattintson a **tárolási** lapon > **tárhelybesorolások leképezése**.
4. Az a **tárhelybesorolások leképezése** lapon jelölje be a besorolások a forrás és cél VMM-kiszolgálók. A beállítások mentéséhez.

    ![Válasszon egy célhálózatot](./media/site-recovery-vmm-to-vmm-classic/storage-mapping.png)

## <a name="step-7-enable-virtual-machine-protection"></a>7. lépés: Virtuális gép védelmének engedélyezése
A kiszolgálók, felhők és hálózatok megfelelő konfigurálását követően engedélyezheti a felhőben működő virtuális gépek védelmét.

1. Az a **virtuális gépek** lapra, ahol a virtuális gép is található a felhőben, majd **engedélyezni a védelmet** > **adja hozzá a virtuális gépek**.
2. A felhőben működő virtuális gépek listájából válassza ki azt, amelyet védelemmel szeretne ellátni.

    ![A virtuális gép védelmének engedélyezése](./media/site-recovery-vmm-to-vmm-classic/enable-protection.png)
3. A Védelemengedélyezési művelet előrehaladását úgy követheti nyomon a **feladatok** lapon, beleértve a kezdeti replikálást. A védelem véglegesítése feladat futtatása után a virtuális gép készen áll a feladatátvételre. A védelem engedélyezését, valamint a virtuális gép replikálását követően megtekintheti őket az Azure-ban.

    ![A virtuális gép védelme feladat](./media/site-recovery-vmm-to-vmm-classic/vm-jobs.png)

> [!NOTE]
> A VMM-konzol virtuális gépek védelmét is engedélyezheti. Kattintson a **Védelemengedélyezési** eszköztárán a **Azure Site Recovery** a virtuális gép tulajdonságok lapján.
>
>

### <a name="on-board-existing-virtual-machines"></a>A helyi meglévő virtuális gépek
Ha meglévő virtuális gépek a VMM Alkalmazásban – a Hyper-V replika replikál szüksége érheti őket az Azure Site Recovery általi védelmet az alábbiak szerint:

1. Ellenőrizze, hogy rendelkezik-e az elsődleges és másodlagos felhők. Győződjön meg arról, hogy az elsődleges felhőben található-e a Hyper-V kiszolgáló, a meglévő virtuális gépet szolgáltató, hogy a Hyper-V kiszolgáló, a replika virtuális gépet szolgáltató a másodlagos felhőben található. Győződjön meg arról, hogy a felhő védelmi beállításainak konfigurálását. A beállítások meg kell felelnie a Hyper-V replika jelenleg konfigurált. Ellenkező esetben a virtuális gép replikációs előfordulhat, hogy nem működik megfelelően.
2. Engedélyezze az elsődleges virtuális gép védelmét. Az Azure Site Recovery és a VMM biztosítja, hogy az azonos másodpéldány-állomás és a virtuális gép észlel, és Azure Site Recovery lesznek ismét felhasználni, és a felhő konfigurálása során konfigurált beállítások segítségével replikálási szolgáltatás használatához.

## <a name="test-your-deployment"></a>A központi telepítés tesztelése
Az üzemelő példány el egyetlen virtuális gépre, a feladatátvételi teszt futtatása vagy több virtuális gépet tartalmazó helyreállítási tervet létrehozni és futtassa a terv feladatátvételi tesztjét.  A feladatátvételi teszt segítségével elkülönített hálózatban próbálhatja ki a feladatátvételi és helyreállítási mechanizmusokat.

### <a name="create-a-recovery-plan"></a>Helyreállítási terv létrehozása
1. Az a **helyreállítási tervek** lapra, majd **helyreállítási terv létrehozása**.
2. Adja meg a helyreállítási tervben és a forrás és cél VMM-kiszolgáló nevét. A forráskiszolgáló kell rendelkeznie, amelyek engedélyezve vannak a feladatátvétel és a helyreállítási virtuális gépek. Válassza ki **Hyper-V** csak felhő sincs beállítva a Hyper-V replikáció megtekintéséhez.

    ![Helyreállítási terv létrehozása](./media/site-recovery-vmm-to-vmm-classic/recovery-plan1.png)
3. A **válassza ki a virtuális gép**, jelölje ki a replikációs csoportokat. A replikációs csoporthoz társított összes virtuális gépet választott lesz, és a rendszer a helyreállítási terv hozzá. Ezek a virtuális gépek a helyreállítási terv alapértelmezett csoportjába, az 1. csoportba kerülnek. több olyan csoportot adhat hozzá, ha szükséges. Vegye figyelembe, hogy után a replikáció virtuális gépeket a helyreállítási terv csoportok megfelelően fog elindításához.

    ![Virtuális gépek hozzáadása](./media/site-recovery-vmm-to-vmm-classic/recovery-plan2.png)

A helyreállítási terv létrehozása után megjelenik a listában a a **helyreállítási tervek** fülre.

### <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása
1. A **Helyreállítási tervek** lapon válassza ki a kívánt tervet, majd kattintson a **Feladatátvételi teszt** lehetőségre.
2. Az a **erősítse meg a feladatátvételi teszt** lapon jelölje be **nincs**. Vegye figyelembe, hogy ez a beállítás engedélyezve a sikertelen replika virtuális gépeket nem fog csatlakozni egyetlen hálózathoz sem. Ez fogja tesztelni, hogy a virtuális gép átadja a feladatokat, mint a várt, de nem vizsgálja meg a replikációs hálózati környezetben. Olvassa el [feladatátvételi teszt futtatása](site-recovery-failover.md) különböző hálózati beállítások használatával kapcsolatos további részletekért.
3. A teszt virtuális gép ugyanazon a gazdagépen, amelyen a replika virtuális gép található a gazdagép létrejön. Kerül, ahol a replika virtuális gép is található azonos felhőben.

### <a name="run-a-recovery-plan"></a>A helyreállítási terv futtatása
Replikálás után a replika virtuális gép esetleg nincs olyan IP-cím ugyanaz, mint az elsődleges virtuális gép IP-címét. Virtuális gépek frissíti a DNS-kiszolgálót használják indítása után. Frissítse a DNS-kiszolgáló egy időben történő frissítés biztosításához parancsfájlt is hozzáadhat.

#### <a name="script-to-retrieve-the-ip-address"></a>A parancsfájl az IP-cím beolvasása
Futtassa a parancsfájlpéldát az IP-cím beolvasása.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="script-to-update-dns"></a>Parancsfájl DNS frissítéséhez
Ez a parancsfájlpélda, DNS, adja meg az IP-cím, a korábbi minta parancsfájlt lekért frissítéséhez futtassa.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="privacy-information-for-site-recovery"></a>A Site Recovery vonatkozó adatvédelmi információ
Ez a témakör további információkat a Microsoft Azure Site Recovery szolgáltatáshoz ("szolgáltatás"). Az adatvédelmi nyilatkozat a Microsoft Azure-szolgáltatások megtekintése: a [Microsoft Azure adatvédelmi nyilatkozata](http://go.microsoft.com/fwlink/?LinkId=324899)

**A szolgáltatás: regisztráció**

* **Működés**:-kiszolgáló regisztrálása a szolgáltatással, hogy a virtuális gépek védhetők.
* **Összegyűjtött adatok**: után a szolgáltatás regisztrációja gyűjti, feldolgozza, és továbbítja a felügyeleti tanúsítvány adatait van kijelölve, adja meg a VMM-kiszolgáló szolgáltatás nevét, és a név vész-helyreállítási VMM-kiszolgálóról a virtuálisgép-felhők a VMM-kiszolgálón.
* **Az információk felhasználása**:

  * Felügyeleti tanúsítvány – azonosításához, és a regisztrált VMM-kiszolgálót a szolgáltatáshoz való hozzáférés hitelesítéséhez használatos. A szolgáltatás a tanúsítvány nyilvános kulcsú része használja a biztonságos egy jogkivonatot, amely csak a regisztrált VMM-kiszolgálót is hozzáférhetnek. A kiszolgáló kell használnia a token a szolgáltatások eléréséhez.
  * A VMM-kiszolgáló nevét – a VMM-kiszolgáló nevének megadása kötelező, és a megfelelő VMM-kiszolgálót, amelyiken a felhők találhatók kommunikálni.
  * A VMM-kiszolgáló nevét a felhő – a felhő nevének megadása kötelező, ha a szolgáltatás felhőalapú, az alábbiakban funkció párosítást/párosításának megszüntetéséhez. Ha úgy dönt, hogy a felhő egy másik felhőt, a helyreállítás adatközpont elsődleges adatközpontból párba, a helyreállítási adatközpontból összes felhő nevei jelennek meg.
* **Választás**: Ez az információ a szolgáltatás regisztrációs folyamat nagyon fontos részét képezik, mivel segíti, és a szolgáltatás az Azure Site Recovery általi védelem, valamint arra, hogy azonosítsa a megfelelő kívánt VMM-kiszolgáló azonosítására regisztrált VMM-kiszolgálót. Ha nem kívánja elküldeni ezeket az információkat a szolgáltatásnak, ne használja ezt a szolgáltatást. Ha regisztrálja a kiszolgálót, majd később meg szeretne regisztrációjának megszüntetésére törölni kell a VMM-kiszolgáló adatait a szolgáltatás portálján (amely az Azure-portál) is megteheti.

**A szolgáltatás: Azure Site Recovery általi védelem engedélyezése**

* **Működés**: az Azure Site Recovery Provider a VMM-kiszolgálón telepítve a szolgáltatással folytatott kommunikáció az átjáró. A szolgáltató nem található a VMM folyamat egy dinamikus csatolású függvénytár (DLL). A szolgáltató telepítése után a "Adatközpont-helyreállítási" lekérdezi engedélyezve van a VMM felügyeleti konzolon. Felhő minden új vagy meglévő virtuális gépek engedélyezheti a virtuális gép védelme "Datacenter helyreállítási" nevű tulajdonságot. Ha ez a tulajdonság értéke, a szolgáltató küld nevének és a virtuális gép Azonosítójának a szolgáltatásnak. A virtuális védelem engedélyezve van a Windows Server 2012 vagy Windows Server 2012 R2 Hyper-V replikációs technológiával. A virtuális gép adatok replikálását, egy Hyper-V gazdagépről egy másikra (általában a különböző "helyreállítási" adatközpontban található).
* **Összegyűjtött adatok**: A szolgáltatás gyűjti, folyamatokat, és továbbítja a metaadatok a virtuális gépet, mely tartalmazza a nevét, a azonosító, a virtuális hálózat és a felhő neve, amelyhez tartozik.
* **Az információk felhasználása**: A szolgáltatás használja a fenti adatok feltöltése a virtuális gép adatait a portálon.
* **Választás**: Ez a szolgáltatás nagyon fontos részét képezik, és nem kapcsolható ki. Ha nem szeretné ezt az információt a szolgáltatásnak küldött, nem engedélyezi az Azure Site Recovery általi védelmet a virtuális gépek. Vegye figyelembe, hogy a szolgáltatás a szolgáltató által küldött minden adathoz HTTPS-KAPCSOLATON keresztül zajlik.

**A szolgáltatás: A helyreállítási terv**

* **Működés**: Ez a szolgáltatás segít, hogy a "helyreállítási" data center vezénylési tervének létrehozása. Megadhatja, hogy a sorrendet, amelyben a virtuális gépek vagy virtuális gépek csoportjának el kell indítani a helyreállítási helyen. Futtatható vagy bármilyen, kézi művelet végrehajtását, minden egyes virtuális gép helyreállítási időpontjában kell automatizált parancsfájlokat is megadható. (A következő szakaszban ismertetett) feladatátvételi általában elindul a helyreállítási terv szinten koordinált helyreállítási.
* **Összegyűjtött adatok**: A szolgáltatás gyűjti, folyamatokat, és továbbítja a metaadatok a helyreállítási terv, beleértve a virtuális gép metaadatait, és bármilyen automatizálási parancsfájlokat és a manuális műveletet megjegyzések metaadatait.
* **Az információk felhasználása**: A fent leírt metaadatok segítségével hozhatók létre a helyreállítási terv a a szolgáltatás portálján.
* **Választás**: Ez a szolgáltatás nagyon fontos részét képezik, és nem kapcsolható ki. Ha nem szeretné ezt az információt a szolgáltatásnak küldött, nem hozhat létre helyreállítási tervek a szolgáltatásban.

**Szolgáltatás: A hálózatleképezés**

* **Működés**: Ez a funkció lehetővé teszi a hálózati adatait az elsődleges adatközpont a helyreállítás adatközpont leképezését. Helyreállításakor a virtuális gépek vannak a helyreállítási helyen, ez a leképezés segítséget nyújt a hálózati kapcsolatot lehessen rájuk vonatkozóan.
* **Összegyűjtött adatok**: a hálózati leképezési szolgáltatás részeként a gyűjti, dolgozza fel, és továbbítja a minden hely (elsődleges és datacenter) tartozó logikai hálózatok metaadatait.
* **Az információk felhasználása**: A szolgáltatás használ a metaadatok feltöltése a portálon ahol is leképezheti a hálózati.
* **Választás**: Ez a szolgáltatás nagyon fontos részét képezik, és nem kapcsolható ki. Ha nem szeretné ezt az információt a szolgáltatásnak küldött, ne használjon a hálózati leképezés funkciója.

**A szolgáltatás: Feladatátvevő - tervezett, nem tervezett, teszt**

* **Működés**: Ez a szolgáltatás segít a virtuális gép egy VMM által felügyelt adatközpontból egy másik VMM-vezérelt adatközpont. A feladatátvételi művelet akkor váltódik ki, a felhasználók a portálon. Lehetséges feladatátvevő okai például egy nem tervezett esemény (például egy természetes disaster0; esetén a tervezett események (például datacenter terheléselosztási); a feladatátvételi tesztet (például a helyreállítási terv részletes).

A VMM-kiszolgálón a szolgáltató lekérdezi az eseményben a szolgáltatás értesítést kap, és a feladatátvételi művelet végrehajtása a Hyper-V gazdagép VMM felületeken keresztül. A Windows Server 2012 vagy Windows Server 2012 R2 Hyper-V replikációs technológiával tényleges feladatátvételt a virtuális gép egy Hyper-V gazdagépről egy másikra (általában a különböző "helyreállítási" adatközpont futtató) kezeli. Miután a feladatátvétel befejeződött, a "helyreállítási" adatközpont VMM-kiszolgálón telepített szolgáltató a sikeres adatokat küld a szolgáltatást.

* **Összegyűjtött adatok**: A szolgáltatás a fenti adatokat használja a feladatátvételi művelet adatait a portálon állapotának feltöltéséhez.
* **Az információk felhasználása**: A szolgáltatás által használt a fenti adatokat az alábbiak szerint:

  * Felügyeleti tanúsítvány – azonosításához, és a regisztrált VMM-kiszolgálót a szolgáltatáshoz való hozzáférés hitelesítéséhez használatos. A szolgáltatás a tanúsítvány nyilvános kulcsú része használja a biztonságos egy jogkivonatot, amely csak a regisztrált VMM-kiszolgálót is hozzáférhetnek. A kiszolgáló kell használnia a token a szolgáltatások eléréséhez.
  * A VMM-kiszolgáló nevét – a VMM-kiszolgáló nevének megadása kötelező, és a megfelelő VMM-kiszolgálót, amelyiken a felhők találhatók kommunikálni.
  * A VMM-kiszolgáló nevét a felhő – a felhő nevének megadása kötelező, ha a szolgáltatás felhőalapú, az alábbiakban funkció párosítást/párosításának megszüntetéséhez. Ha úgy dönt, hogy a felhő egy másik felhőt, a helyreállítás adatközpont elsődleges adatközpontból párba, a helyreállítási adatközpontból összes felhő nevei jelennek meg.
* **Választás**: Ez a szolgáltatás nagyon fontos részét képezik, és nem kapcsolható ki. Ha nem szeretné ezt az információt a szolgáltatásnak küldött, ne használja ezt a szolgáltatást.

## <a name="next-steps"></a>Következő lépések
Ellenőrizze, hogy a környezetében a várt módon működik, feladatátvételi teszt futtatását követően [megismerése](site-recovery-failover.md) feladatátvételek különböző típusú.
