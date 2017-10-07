---
title: "Hyper-V virtuális gépek VMM tooa másodlagos hely (a klasszikus Azure portál) aaaReplicate |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooreplicate Hyper-V virtuális gépek a VMM-felhők tooa VMM másodlagos hely az Azure Site Recovery szolgáltatással."
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
ms.openlocfilehash: fe551e1f741579044f540ea0e50a6e36d48133ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site"></a>Hyper-V virtuális gépek VMM felhők tooa másodlagos VMM-hely replikálása
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Klasszikus portál](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

hello Azure Site Recovery szolgáltatás hozzájárul tooyour az üzletmenet folytonosságának és vész helyreállítási (BCDR) stratégia megvalósításában replikációjának, feladatátvételének és helyreállításának virtuális gépek és fizikai kiszolgálók által. Gépek replikált tooAzure vagy tooa másodlagos helyszíni adatközpontba lehet. A szolgáltatás gyors áttekintéséhez olvassa el az [Azure Site Recovery szolgáltatás mibenlétével foglalkozó](site-recovery-overview.md) cikket.

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan tooreplicate Hyper-V virtuális gépek Hyper-v gazdagép VMM felhők toosecondary VMM hely Azure Site Recovery segítségével a felügyelt kiszolgálókon.

hello cikk Előfeltételek tartalmaz, bemutatja, hogyan telepítheti a tooset be a Site Recovery-tároló, hello Azure Site Recovery Provider a forrás és cél VMM-kiszolgálók, hello kiszolgálók regisztrálja hello tárolóban, adja meg a VMM-felhő védelmi beállításait, és engedélyez Hyper-V virtuális gépek védelmét. Végül tesztelési hello feladatátvételi toomake meg arról, hogy minden a várt módon működik.

Ez a cikk vagy a hello hello alsó megjegyzések vagy kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Architektúra
hello a következő ábrán hello különböző kommunikációs csatornák és a vezénylési és a replikálás az Azure Site Recovery által használt portok

![E2E topológia](./media/site-recovery-vmm-to-vmm-classic/e2e-topology.png)

## <a name="before-you-start"></a>Előkészületek
Győződjön meg arról, hogy az előfeltételek teljesülnek:

| **Előfeltételek** | **Részletek** |
| --- | --- |
| **Azure** |Szüksége van egy [Microsoft Azure](https://azure.microsoft.com/)-fiókra. Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is. [További információk](https://azure.microsoft.com/pricing/details/site-recovery/) a Site Recovery díjszabásáról. |
| **VMM** |Legalább egy VMM-kiszolgáló van szüksége.<br/><br/>hello VMM-kiszolgálón legalább futnia kell a System Center 2012 SP1 hello legújabb kumulatív frissítéseket.<br/><br/>Ha azt szeretné, hogy egyetlen VMM-kiszolgálóval védelem tooset, legalább két felhők hello kiszolgálón konfigurálni kell.<br/><br/>Ha azt szeretné, hogy a két VMM-kiszolgáló toodeploy védelmet, minden kiszolgáló rendelkeznie kell legalább egy felhőnek VMM-kiszolgálón hello elsődleges tooprotect, valamint egy felhőnek hello másodlagos VMM-kiszolgáló védelmi és helyreállítási kívánt toouse<br/><br/>Az összes VMM-felhő hello beállítása a Hyper-V-kapacitásprofilt kell rendelkeznie.<br/><br/>hello tooprotect kívánt forrásfelhőnek legalább egy VMM-gazdagépcsoportot kell tartalmaznia. |
| **Hyper-V** |Hello elsődleges és másodlagos VMM-gazdagépcsoportot lévő egy vagy több Hyper-V gazdakiszolgálók, és minden Hyper-V gazdakiszolgálón futó virtuális gépek egy vagy több szükséges.<br/><br/>hello gazdagép és a cél Hyper-V kiszolgálók futnia kell az legalább hello Hyper-V szerepkör és a Windows Server 2012 és a legújabb frissítések telepítve rendelkezik hello.<br/><br/>A virtuális gépeket tartalmazó Hyper-V server kívánt tooprotect egy VMM-felhőben kell lennie.<br/><br/>Ha a Hyper-V egy fürtben fut, vegye figyelembe, hogy a fürtszervező nem hozza létre automatikusan Ha egy statikus IP cím alapú fürtöt. Manuálisan kell tooconfigure hello fürtszervező. [További információkat](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) Aidan Finn blogbejegyzésében talál. |
| **Hálózatleképezés** |Hálózati leképezési toomake arról, hogy a replikált virtuális gépek optimális kerülnek, a másodlagos Hyper-V gazdakiszolgálókra feladatátvétel után, és, hogy csatlakozhassanak tooappropriate Virtuálisgép-hálózatokat is konfigurálhat. Ha nem konfigurálja a hálózatra való leképezést, a replika virtuális gépek nem csatlakoztatott tooany hálózati feladatátvételt követően.<br/><br/>tooset fel a hálózatra való leképezést a telepítés során ellenőrizze, hogy hello virtuális gépek hello forrás Hyper-V gazdakiszolgálón futó csatlakoztatott tooa VMM Virtuálisgép-hálózatot. Erre a hálózatra kell lennie a logikai hálózathoz csatolt tooa társított hello felhő. < br /<br/>hello célfelhő hello másodlagos VMM-kiszolgálón a helyreállításhoz használt rendelkeznie kell a megfelelő Virtuálisgép-hálózatot, és megfelelő hello célfelhő társított logikai hálózati csatolt tooa pedig kell. |
| **Tárolási leképezése** |Alapértelmezés szerint a replikált virtuális gép egy forrás Hyper-V gazdakiszolgálón futó kiszolgáló tooa cél Hyper-V gazdagép, a replikált adatok tárolódik hello cél Hyper-V gazdagépet a Hyper-V kezelőjében a feltüntetett hello alapértelmezett helye. Pontosabban a replikált adatok tárolására konfigurálhatja a tárolási leképezése<br/><br/> tooconfigure tárolási hozzárendelést, meg kell tooset mentése tárhelybesorolások hello forrás és cél VMM-kiszolgálók központi telepítésének megkezdése előtt. |

## <a name="step-1-create-a-site-recovery-vault"></a>1. lépés: Site Recovery-tároló létrehozása
1. Jelentkezzen be toohello [kezelési portál](https://portal.azure.com) tooregister kívánt hello VMM-kiszolgálóról.
2. Bontsa ki a **adatszolgáltatások** > **Recovery Services** kattintson **Site Recovery-tároló**.
3. Kattintson az **Új létrehozása** > **Gyorslétrehozás** elemre.
4. A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban.
5. A **régió** válasszon hello hello tároló földrajzi régióját. támogatott toocheck régiók, tekintse meg a cikknek a földrajzi elérhetőséggel [Azure Site Recovery – díjszabás](http://go.microsoft.com/fwlink/?LinkId=389880).
6. Kattintson a **Tároló létrehozása** elemre.

    ![tároló létrehozása](./media/site-recovery-vmm-to-vmm-classic/create-vault.png)

Ellenőrizze, hogy hello tároló hello állapotsorban hozták létre. hello tároló tulajdonosaként **aktív** a hello Recovery Services főoldalán.

## <a name="step-2-generate-a-vault-registration-key"></a>2. lépés: A tároló regisztrációs kulcsának létrehozása
Hozzon létre egy regisztrációs kulcsot hello tárolóban lévő állapottal. Miután hello Azure Site Recovery Provider letöltése, és telepítse a VMM-kiszolgálón hello, a kulcs tooregister hello VMM-kiszolgáló hello tárolóban fogja használni.

1. A hello **Recovery Services** hello tároló tooopen hello gyors kezdés lapon kattintson. Gyors üzembe helyezési hello ikon segítségével is megnyithatja.

    ![Első lépések ikon](./media/site-recovery-vmm-to-vmm-classic/quick-start-icon.png)
2. Hello legördülő listában válassza ki a **két helyszíni VMM-helyek közötti**.
3. A **előkészítése a VMM-kiszolgálók**, kattintson a **Generate regisztrációs kulcsot tartalmazó fájlt**. hello kulcsfájl automatikusan létrejön-e, és ezt követően 5 napig érvényes. Ha nem éri el az Azure portál hello hello VMM-kiszolgálóról kell toocopy toohello fájlkiszolgálót.

    ![Regisztrációs kulcs](./media/site-recovery-vmm-to-vmm-classic/register-key.png)

## <a name="step-3-install-hello-azure-site-recovery-provider"></a>3. lépés: Hello Azure Site Recovery Provider telepítése
1. A hello **gyors üzembe helyezés** lap **előkészítése a VMM-kiszolgálók**, kattintson a **töltse le a Microsoft Azure Site Recovery Provider telepítése a VMM Server** tooobtain hello legújabb hello szolgáltató telepítési fájl verziója.
2. Futtassa ezt a fájlt hello forrás VMM-kiszolgálón.

   > [!NOTE]
   > Ha a VMM telepítve van a fürtben, és telepítése hello szolgáltató hello az első alkalommal telepítse azt aktív csomópontra, és Befejezés hello telepítési tooregister hello VMM-kiszolgáló hello tárolóban lévő állapottal. Majd telepítse hello szolgáltató hello más csomópontokra. Vegye figyelembe, hogy ha frissít, akkor meg kell tooupgrade az összes olyan csomóponton, mivel azok kell minden szolgáltató hello futó szolgáltató azonos verziója hello.
   >
   >
3. hello telepítő does néhány **Előfeltételek ellenőrzése** és a kérelmek engedély toostop hello a VMM szolgáltatás toobegin Provider telepítőjét. a VMM szolgáltatás hello automatikusan újraindul a telepítő befejezése után. A telepítése a VMM-fürthöz lesz kéri toostop hello fürtszerepkört.
4. A **Microsoft Update** lapon kérheti a frissítések telepítését. Ez a beállítás engedélyezett frissítéseket telepíti tooyour Microsoft Update-szabályzatnak megfelelően.

    ![Microsoft Update](./media/site-recovery-vmm-to-vmm-classic/ms-update.png)
5. hello telepítési helyére telepíti értéke túl**<SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin**. Kattintson a hello telepítése gomb toostart hello szolgáltató telepítése.

    ![Telepítési hely](./media/site-recovery-vmm-to-vmm-classic/install-location.png)
6. Szolgáltató van telepítve hello után kattintson a **regisztrálása** tooregister hello server hello tárolóban lévő állapottal.

    ![Telepítés kész](./media/site-recovery-vmm-to-vmm-classic/install-complete.png)
7. A **tároló neve**, ellenőrizze, melyik hello a kiszolgálót regisztrálni fogja hello tároló hello nevére. Kattintson a *Tovább* gombra.

    ![Kiszolgáló regisztrációja](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)
8. A **internetkapcsolat** adjon meg hello VMM futó Provider hogyan hello server toohello Internet csatlakozik. Válassza ki **csatlakozás meglévő proxybeállításokkal** toouse hello alapértelmezett internetkapcsolati beállításait hello kiszolgálón konfigurált.

    ![Internetbeállítások](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

   * Ha azt szeretné, hogy toouse egyéni proxyt érdemes beállítania, hello szolgáltató telepítése előtt. Ha egyéni proxyt a beállításokat egy teszt lefuttatásával toocheck hello proxykapcsolatot.
   * Ha egyéni proxyt használ, vagy az alapértelmezett proxy hitelesítést igényel kell tooenter hello proxy adatait, beleértve a hello proxy címét és portszámát.
   * A következő URL-címek hello VMM-kiszolgáló és a Hyper-v-gazdagépek hello elérhetőnek kell lennie
     * *.hypervrecoverymanager.windowsazure.com
     * *.accesscontrol.windows.net
     * *.backup.windowsazure.com
     * *.blob.core.windows.net
     * *.store.core.windows.net
   * Ismertetett hello IP-címek engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653) és a HTTPS (443) protokollt. IP-címtartományok toowhite-lista hello Azure-régió, toouse és az USA nyugati régiója megtervezése kellene lennie.
   * Ha egyéni proxyt használ a VMM RunAs-fiókot (DRAProxyAccount) használatával jön létre automatikusan a megadott hello proxy hitelesítő adatokat. Hello-proxy kiszolgáló konfigurálása, így a fiók elvégezhesse a hitelesítést. hello VMM RunAs-fiók beállításait hello VMM-konzolon módosíthatja. toodo a, nyissa meg hello **beállítások** munkaterületet, bontsa ki a **biztonsági**, kattintson a **futtató fiókok**, majd módosítsa a draproxyaccount jelszavát hello jelszó. Szüksége lesz toorestart hello VMM szolgáltatást, hogy ez a beállítás lép érvénybe.
9. A **regisztrációs kulcs**, jelölje be az Azure Site Recoveryből letöltött, és másolja a toohello VMM-kiszolgáló hello kulcs.
10. hello titkosítási beállítást csak akkor használja, ha a Hyper-V virtuális gépek a VMM-felhők tooAzure replikál. Ha tooa másodlagos helyre replikál nem használható.
11. A **kiszolgálónév**, adjon meg egy rövid nevet tooidentify hello VMM-kiszolgáló hello tárolóban lévő állapottal. Adja meg a fürt konfigurációjába hello VMM-fürtszerepkör nevét.
12. A **Synchronize cloud metaadatok** válassza ki, hogy a VMM-kiszolgálón hello hello tárolóban futó összes felhő toosynchronize metaadatok. Ez a művelet csak egyszer minden kiszolgálón toohappen van szüksége. Ha nem szeretné, toosynchronize összes felhő, hagyja bejelölve ezt a beállítást, és szinkronizálja egyenként a hello felhőtulajdonságok hello VMM-konzolon.
13. Kattintson a **következő** toocomplete hello folyamat. A regisztrációt követően az Azure Site Recovery által lekéri metaadatait hello VMM-kiszolgálóról. hello kiszolgálóhoz **VMM-kiszolgálókon** > **kiszolgálók** hello tárolóban lévő állapottal.

    ![Kiszolgálók](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

### <a name="command-line-installation"></a>Parancssori telepítés
hello Azure Site Recovery Provider hello parancssorból is telepíthető. Ez a módszer egy Server CORE a Windows Server 2012 R2 használt tooinstall hello szolgáltató is lehet.

1. Töltse le a hello szolgáltató telepítési és a regisztrációs kulcs tooa mappát. A mappa lehet például a következő: C:\ASR.
2. Hello System Center Virtual Machine Manager szolgáltatás leállítása
3. Nyerje ki a hello szolgáltató installer parancssort ezek a parancsok futtatásával **rendszergazda** jogosultságokkal:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Telepítse a szolgáltató hello futtatásával:

        C:\ASR> setupdr.exe /i
5. Hello szolgáltató regisztrálása futtatásával:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>     

Ha hello paraméterek a következők:

* **/ Credentials**: kötelező paraméter, amely hello helyet, mely hello a regisztrációs kulcs fájlját található  
* **/ FriendlyName**: hello Hyper-V gazdakiszolgáló hello Azure Site Recovery portálon megjelenő nevét hello paramétert kötelező megadni.
* **/ EncryptionEnabled**: nem kötelező paraméter csak a hello VMM tooAzure forgatókönyv toouse kell, ha a virtuális gépek Azure-ban aktívan titkosítási kell. Győződjön meg arról, hogy hello hello fájl nevét, adja meg, hogy rendelkezik egy **.pfx** bővítmény.
* **/ proxyaddress**: nem kötelező paraméter, amely hello hello proxykiszolgáló címét.
* **/ proxyport**: nem kötelező paraméter, amely hello proxykiszolgáló hello port.
* **/ proxyusername**: nem kötelező paraméter, amely hello Proxy felhasználónevét határozza meg, (Ha a proxy hitelesítést igényel).
* **/ proxypassword**: nem kötelező paraméter meghatározza, hogy a jelszó hello hello proxykiszolgáló hitelesítéséhez (Ha a proxy hitelesítést igényel).  

## <a name="step-4-configure-cloud-protection-settings"></a>4. lépés: Konfigurálja a felhő védelmi beállításait
Miután a VMM-kiszolgáló regisztrálva van, konfigurálhatja a felhő védelmi beállításait. Ha engedélyezte a hello beállítás **Synchronize cloud data hello tárolóban** hello VMM-kiszolgálón futó összes felhő hello fog megjelenni, ha telepítve hello szolgáltató **védett elemek** lapon hello tárolóban lévő állapottal. Ha Ön nem képes szinkronizálni az adott felhő Azure Site Recovery a hello **általános** lap hello felhő tulajdonságok lapján hello VMM-konzolon.

![Közzétett felhő](./media/site-recovery-vmm-to-vmm-classic/clouds-list.png)

1. Hello gyors kezdés lapon kattintson **védelem beállítása a VMM-felhőkben**.
2. A hello **VMM-Felhőkben** lapra, válassza ki a kívánt tooconfigure, és nyissa meg toohello hello felhő **konfigurációs** fülre.
3. A **cél**, jelölje be **VMM**.
4. A **célhelye**, válassza ki a helyszíni VMM-kiszolgáló, amely kezeli a használni kívánt helyreállítási toouse hello felhő hello.
5. A **céloz felhő**, válassza ki a használni kívánt toouse hello forrásfelhőben található virtuális gépek feladatátvétele hello célfelhőt. Vegye figyelembe:

   * Azt javasoljuk, hogy bejelöli-e a megcélzott felhőt, amely megfelel-e helyreállítási hello virtuális gépek fogja védeni.
   * Felhő csak a következőkhöz tartozhatnak tooa egyetlen felhőpárnál – egy elsődleges vagy a megcélzott felhőt.
6. A **másolás gyakorisága**, adja meg, milyen gyakran szinkronizálja az adatokat a 5he forrása és célja között. Vegye figyelembe, hogy ez a beállítás csak megfelelő hello Hyper-V gazdagépen futó Windows Server 2012 R2 esetén. A többi kiszolgáló öt perc az alapértelmezett beállítás használatos.
7. A **további helyreállítási pontok**, adja meg, hogy toocreate további pontok. hello alapértelmezett nulla érték azt jelzi, hogy csak hello legfrissebb helyreállítási pontot az elsődleges virtuális gépen tárolt replika gazdakiszolgálón. Vegye figyelembe, hogy több helyreállítási pont engedélyezése igényel további tárhely hello minden helyreállítási ponton tárolt pillanatképek. Alapértelmezés szerint helyreállítási pontokat óránként létrehozott, így az egyes helyreállítási pontok tartalmaz egy órányi adat tekinthető meg. hello helyreállítási pont érték hello VMM-konzol hello virtuális géphez hozzárendelt nem lehet kisebb, mint hello érték hozzárendelt hello Azure Site Recovery konzolján.
8. A **alkalmazáskonzisztens pillanatképek gyakorisága**, adja meg, milyen gyakran toocreate alkalmazáskonzisztens pillanatképeket. Hyper-V két különböző használ – standard pillanatkép, amely lefedő növekményes pillanatképet hello teljes virtuális gép, és egy alkalmazáskonzisztens pillanatkép, amely időpontban pillanatképet készít a hello alkalmazásadatok hello virtuális gépen belül. Alkalmazáskonzisztens pillanatképek a kötet árnyékmásolata szolgáltatás (VSS) tooensure, amely az alkalmazások konzisztens állapotban legyenek, hello pillanatkép készítésének időpontjában. Vegye figyelembe, hogy ha engedélyezi az alkalmazáskonzisztens pillanatképeket, az hatással hello forrás virtuális gépeken futó alkalmazások teljesítményére. Győződjön meg arról, hogy hello beállított érték kisebb, mint hello további helyreállítási pontok száma.

    ![Védelmi beállítások konfigurálása](./media/site-recovery-vmm-to-vmm-classic/cloud-settings.png)
9. A **adatátvitel tömörítésének**, adja meg, hogy van-e a replikált adatok tömörített.
10. A **hitelesítési**, adja meg, hogyan hello elsődleges és a helyreállítási Hyper-V gazdakiszolgálók közötti forgalom hitelesítése. Válassza ki a HTTPS, hacsak nincs konfigurálva a Kerberos-környezet működőképes. Az Azure Site Recovery automatikusan HTTPS-hitelesítési tanúsítványok konfigurálja. Manuális beállításokra nincs is szükség. Kerberos csak akkor válassza, ha egy Kerberos-jegyet hello kiszolgálók a kölcsönös hitelesítéshez használható. Alapértelmezés szerint a Windows tűzfal hello a Hyper-V gazdakiszolgálókra hello port 8083 és 8084 (a tanúsítványok) fognak megnyílni. Vegye figyelembe, hogy ez a beállítás csak a Windows Server 2012 R2 rendszeren futó Hyper-V gazdakiszolgálók szükséges.
11. A **Port**, hello portszám mely hello forrás módosítása, és a gazdagép számítógépek figyelési replikációs cél. Például hello beállítás előfordulhat, hogy módosítsa, ha azt szeretné, hogy tooapply szolgáltatásminőség (QoS) a hálózati sávszélesség-szabályozási a replikációs forgalmat. Ellenőrizze, hogy hello a portot más alkalmazás nem használja, és hogy meg nyitva hello tűzfal beállításait.
12. A **replikációs mód**, adja meg a forrás tootarget helyekről származó adatok kezdeti replikálása hello kezelésének, rendszeres replikáció megkezdése előtt:

    * **Hálózaton keresztül**– hello hálózaton keresztül az adatok másolásának időigényes és erőforrás-igényes lehet. Azt javasoljuk, hogy ezt a beállítást használja, ha hello felhő viszonylag kis virtuális merevlemezzel rendelkező virtuális gépet tartalmaz, és ha hello elsődleges helyhez csatlakoztatott toohello másodlagos hely nagy sávszélességű kapcsolaton keresztül. Megadhatja, hogy hello példányát kell azonnal elindítani, vagy válassza ki a megfelelő. Ha hálózati replikáció használata esetén ajánlott ütemezni a csúcsidőn.
    * **Kapcsolat nélküli**– Ez a módszer határozza meg, hogy hello a kezdeti replikáció külső adathordozó segítségével történik. Ez akkor hasznos, ha azt szeretné, hogy a hálózati teljesítményt, vagy földrajzilag távoli helyekre tooavoid teljesítménycsökkenést. toouse ezt a módszert hello exportálás helyére hello forrásfelhőnek és hello importálási helyére hello célfelhőt adjon meg. Ha engedélyezi a virtuális gép védelmét, hello virtuális merevlemez-e a megadott másolt toohello exportálási hely. Elküldi a toohello célhelyre, és toohello importálási helyre másolja. hello rendszer másolatok hello importált információk toohello replika virtuális gépeket.
13. Válassza ki **replika virtuális gép törlése** , amely a replika virtuális gép hello toospecify törölni kell, ha kikapcsolja az hello virtuális gép védelmét hello kiválasztásával **hello virtuális gép védelmét törlése**  hello virtuális gépek lap hello felhő tulajdonságainak beállítását. A beállítás engedélyezése, letiltása védelmi hello virtuális gép Azure Site Recovery törlődik, hello virtuális gép hello Site Recovery beállításait törlődnek hello VMM-konzolon, és hello replika törlése.

    ![Védelmi beállítások konfigurálása](./media/site-recovery-vmm-to-vmm-classic/cloud-settings-replica.png)

Hello beállítások mentését követően a feladat jön létre, és figyelni az hello **feladatok** fülre. A replikáció hello VMM-forrásfelhőben található összes Hyper-V gazdakiszolgálók lesz konfigurálva. Felhőbeállítások módosíthatók hello **konfigurálása** fülre. Ha azt szeretné, hogy toomodify hello célhelye vagy célfelhő távolítsa el hello felhő konfigurációját, és az hello felhő konfigurálja újra.

### <a name="prepare-for-offline-initial-replication"></a>Offline kezdeti replikálás előkészítése
A következő műveletek tooprepare offline kezdeti replikálás toodo hello lesz szüksége:

* Hello forráskiszolgálón lesz adja meg egy elérési útja, mely hello származó adatok exportálása akkor kerül sor. Rendeljen teljes hozzáférés az NTFS és a megosztási engedélyek toohello VMM szolgáltatás hello exportálási elérési úton. Hello célkiszolgálón akkor lesz adja meg egy elérési útja, amelyből hello adatok importálása megtörténik. Rendelje hozzá a hello ugyanazokkal az engedélyekkel az importálási útvonalat.
* Ha hello importálása vagy exportálása elérési út megosztása, rendeljen hozzájuk csoporttagságot rendszergazda, a kiemelt felhasználó, a Nyomtatófelelősök vagy a kiszolgáló operátor hello VMM-szolgáltatásfiók hello távoli számítógépen mely megosztott hello található.
* Futtató fiókok tooadd állomások használ, ha a hello importálása és exportálási útvonalakra, rendelje hozzá az olvasási és írási engedélyek toohello futtató fiókokat a VMM-ben.
* Importálás hello és megosztások exportálása nem kell elhelyezni minden olyan számítógépen, használja a Hyper-V gazdagép-kiszolgálón, mert visszacsatolási konfiguráció nem támogatott a Hyper-v.
* Az Active Directory minden Hyper-V gazdakiszolgálón futó virtuális gépeket tartalmazó szeretné, hogy tooprotect, engedélyezheti és konfigurálhatja a korlátozott delegálás tootrust hello távoli számítógépek mely hello importálási és exportálási útvonalak találhatók, az alábbiak szerint:
  1. Hello tartományvezérlőn nyissa meg a **Active Directory – felhasználók és számítógépek**.
  2. Hello konzolfában kattintson **tartománynév** > **számítógépek**.
  3. Kattintson a jobb gombbal a Hyper-V hello futtató kiszolgáló neve > **tulajdonságok**.
  4. A hello **delegálás** lapon kattintson a T**ezen a számítógépen csak a delegálás toospecified szolgáltatások Rozsdás**.
  5. Kattintson a **bármely hitelesítési protokoll**.
  6. Kattintson a **hozzáadása** > **felhasználók és számítógépek**.
  7. Hello típusnév hello számítógép, amelyen hello exportálási elérési útja > **OK**. Elérhető szolgáltatások hello listáról hello CTRL billentyűt lenyomva, majd kattintson a **cifs** > **OK**. Ismételje meg a hello hello számítógép nevét, hogy állomások hello importálási elérési útja. Ismételje meg a Hyper-V-gazdagép további kiszolgálókat a megfelelő.

## <a name="step-5-configure-network-mapping"></a>5. lépés: Hálózatleképezés konfigurálása
1. Hello gyors kezdés lapon kattintson **hálózatok leképezése**.
2. Válassza ki a hello forrás VMM-kiszolgálót szeretné, hogy toomap hálózatok, és majd hello a cél VMM server toowhich hello hálózatok lesz leképezve szövegrészt. hello forrás hálózatok listáját és azok kapcsolódó célhálózatok jelennek meg. Jelenleg nincsenek leképezve hálózatok jelenik meg egy üres érték.
3. Válasszon egy hálózatot a **hálózati forrás** > **térkép**. hello szolgáltatás hello Virtuálisgép-hálózatokat a célkiszolgálón hello észleli, és megjeleníti őket. Kattintson a hello információk ikon következő toohello forrása és célja hálózati nevek tooview hello alhálózatok minden hálózathoz.

    ![Hálózatleképezés konfigurálása](./media/site-recovery-vmm-to-vmm-classic/network-mapping1.png)
4. Hello párbeszédpanelen válassza ki azt a Virtuálisgép-hálózatok hello hello cél VMM-kiszolgálóval.

    ![Válasszon egy célhálózatot](./media/site-recovery-vmm-to-vmm-classic/network-mapping2.png)
5. Amikor kiválaszt egy célhálózatot, hello hello Forráshálózat használó védett felhő jelennek meg. Az elérhető célkiszolgálók hálózatok, amelyek társított használt védelemre hello felhők is látható. Azt javasoljuk, hogy bejelöli-e egy célhálózatot, amely elérhető tooall hello felhők védelmet alkalmaz. Vagy is megnyithatja a VMM-kiszolgáló toohello és hello felhő tulajdonságok tooadd hello logikai hálózat módosítása, hogy szeretné-e toochoose toohello virtuálisgép-hálózat megfelelő.
6. Kattintson a hello pipa toocomplete hello megfeleltetési folyamat. A feladatok elindításának tootrack hello leképezési művelet előrehaladását. Megtekintheti a hello **feladatok** fülre.

## <a name="step-6-configure-storage-mapping"></a>6. lépés: Konfigurálja a tárolási leképezése
Alapértelmezés szerint a replikált virtuális gép egy forrás Hyper-V gazdakiszolgálón futó kiszolgáló tooa cél Hyper-V gazdagép, a replikált adatok tárolódik hello cél Hyper-V gazdagépet a Hyper-V kezelőjében a feltüntetett hello alapértelmezett helye. Pontosabban a replikált adatok tárolására az alábbiak szerint konfigurálhatja tárolási leképezéseket:

1. Tárhelybesorolások adja meg mindkét hello forrás és cél VMM-kiszolgálók. [További információk](https://technet.microsoft.com/library/gg610685.aspx). Besorolások elérhető toohello forrás és cél Hyper-V gazdakiszolgálók kell lennie. Nincs szükség a besorolások toohave hello azonos típusú tárolón. Például a forrás besorolást, amely tartalmazza az SMB megosztások tooa Cél besorolás, amely tartalmazza a CSV-k is leképezheti.
2. Miután besorolások pedig a következők teljesülnek leképezéseket hozhat létre. toodo Ez a hello **gyors üzembe helyezés** lap > **tárolási leképezése**.
3. Kattintson a hello **tárolási** lapon > **tárhelybesorolások leképezése**.
4. A hello **tárhelybesorolások leképezése** lapon jelölje be a besorolások hello forrás és cél VMM-kiszolgálók. A beállítások mentéséhez.

    ![Válasszon egy célhálózatot](./media/site-recovery-vmm-to-vmm-classic/storage-mapping.png)

## <a name="step-7-enable-virtual-machine-protection"></a>7. lépés: Virtuális gép védelmének engedélyezése
Kiszolgálók, felhők és hálózatok megfelelő konfigurálását követően engedélyezheti a hello felhő virtuális gépek védelmét.

1. A hello **virtuális gépek** mely hello a virtuális gép is található hello felhőben lapra, majd **engedélyezni a védelmet** > **adja hozzá a virtuális gépek**.
2. A virtuális gépek hello felhőben hello listájából válassza ki azt szeretné, hogy tooprotect egy hello.

    ![A virtuális gép védelmének engedélyezése](./media/site-recovery-vmm-to-vmm-classic/enable-protection.png)
3. Hello hello a Védelemengedélyezési művelet előrehaladását úgy követheti nyomon **feladatok** lapon, beleértve a kezdeti replikáció hello. Miután hello Védelemvéglegesítési feladatot futtat hello virtuális gép készen áll a feladatátvételre. Miután a védelem engedélyezve van, és a virtuális gép replikálását, képes tooview lesz őket az Azure-ban.

    ![A virtuális gép védelme feladat](./media/site-recovery-vmm-to-vmm-classic/vm-jobs.png)

> [!NOTE]
> Hello VMM-konzol virtuális gépek védelmét is engedélyezheti. Kattintson a **Védelemengedélyezési** hello hello eszköztárán **Azure Site Recovery** hello virtuális gép tulajdonságok lapján.
>
>

### <a name="on-board-existing-virtual-machines"></a>A helyi meglévő virtuális gépek
Ha rendelkezik meglévő virtuális gépek a VMM Alkalmazásban – a Hyper-V replika replikál tooonboard kell őket az Azure Site Recovery általi védelem, az alábbiak szerint:

1. Ellenőrizze, hogy rendelkezik-e az elsődleges és másodlagos felhők. Győződjön meg arról, hogy hello meglévő virtuális gépet szolgáltató hello Hyper-V server hello elsődleges felhőben található hello replika virtuális gépet szolgáltató hello Hyper-V kiszolgálón hello másodlagos felhőben található. Győződjön meg arról, hogy konfigurálta az hello felhő védelmi beállításait. hello beállításait meg kell felelnie a Hyper-V replika jelenleg konfigurált. Ellenkező esetben a virtuális gép replikációs előfordulhat, hogy nem működik megfelelően.
2. Engedélyezze a hello elsődleges virtuális gép védelmét. Az Azure Site Recovery és a VMM biztosítja, hogy hello azonos másodpéldány-állomás és a virtuális gép észlel, és Azure Site Recovery lesz ismét felhasználni, és a felhő konfigurálása során konfigurált hello-beállítások replikálási szolgáltatás használatához.

## <a name="test-your-deployment"></a>A központi telepítés tesztelése
Tervezze meg a központi telepítés futtathat egyetlen virtuális gép feladatátvételi tesztjét, vagy több virtuális gép álló és hello a feladatátvételi teszt futtatása helyreállítási terv létrehozása tootest.  A feladatátvételi teszt segítségével elkülönített hálózatban próbálhatja ki a feladatátvételi és helyreállítási mechanizmusokat.

### <a name="create-a-recovery-plan"></a>Helyreállítási terv létrehozása
1. A hello **helyreállítási tervek** lapra, majd **helyreállítási terv létrehozása**.
2. Adjon meg egy nevet hello helyreállítási tervet, és a forrás és cél VMM-kiszolgálókon. hello forráskiszolgáló kell rendelkeznie, amelyek engedélyezve vannak a feladatátvétel és a helyreállítási virtuális gépek. Válassza ki **Hyper-V** tooview csak felhő sincs beállítva a Hyper-V replikáció.

    ![Helyreállítási terv létrehozása](./media/site-recovery-vmm-to-vmm-classic/recovery-plan1.png)
3. A **válassza ki a virtuális gép**, jelölje ki a replikációs csoportokat. Hello replikációs csoporthoz társított összes virtuális gépet kijelöli, és hozzá toohello helyreállítási terv. A virtuális gépeket adnak toohello helyreállítási terv alapértelmezett csoport – csoport 1. több olyan csoportot adhat hozzá, ha szükséges. Vegye figyelembe, hogy után a replikáció virtuális gépek hello helyreállítási terv csoportok hello sorrendjének megfelelően fog elindításához.

    ![Virtuális gépek hozzáadása](./media/site-recovery-vmm-to-vmm-classic/recovery-plan2.png)

A helyreállítási terv létrehozása után megjelenik a hello hello listában **helyreállítási tervek** fülre.

### <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása
1. A hello **helyreállítási tervek** lapra, válassza ki a hello tervet, és kattintson **feladatátvételi teszt**.
2. A hello **erősítse meg a feladatátvételi teszt** lapon jelölje be **nincs**. Vegye figyelembe, hogy ez a beállítás engedélyezve van, a replika virtuális gépek a feladatátvételt hello nem csatlakoztatott tooany hálózati. Ennek teszteléséhez hello várt feletti virtuális gép leáll, de nem vizsgálja meg a replikációs hálózati környezetben. Olvassa túl[feladatátvételi teszt futtatása](site-recovery-failover.md) kapcsolatos további részletekért toouse különböző alkalmazás hálózati beállításait.
3. hello teszt virtuális gép hello azonos gazdagép mely hello a replika virtuális gép valóban létezik hello gazdagépeként fognak létrejönni. Toohello kerül azonos felhőben mely hello a replika virtuális gép is található.

### <a name="run-a-recovery-plan"></a>A helyreállítási terv futtatása
Után a replikáció hello replika virtuális gép esetleg nincs IP-címet, amely hello hello IP-címe megegyezik az elsődleges virtuális gép hello. Virtuális gépek frissíteni fogja a hello DNS-kiszolgálót használják indítása után. Azt is megteheti egy parancsfájl tooupdate hello DNS-kiszolgáló tooensure egy időben történő frissítést.

#### <a name="script-tooretrieve-hello-ip-address"></a>Parancsfájl tooretrieve hello IP-cím
Ez a minta parancsprogram tooretrieve hello IP-cím futtatásához.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="script-tooupdate-dns"></a>Parancsfájl tooupdate DNS
Ez a minta parancsprogram tooupdate DNS hello IP-cím megadásával futtassa lekért hello korábbi minta parancsfájlt.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="privacy-information-for-site-recovery"></a>A Site Recovery vonatkozó adatvédelmi információ
Ez a témakör további adatvédelmi információ a hello Microsoft Azure Site Recovery szolgáltatás ("szolgáltatás"). tooview hello adatvédelmi nyilatkozat a Microsoft Azure-szolgáltatásokra, tekintse meg a [Microsoft Azure adatvédelmi nyilatkozata](http://go.microsoft.com/fwlink/?LinkId=324899)

**A szolgáltatás: regisztráció**

* **Működés**:-kiszolgáló regisztrálása a szolgáltatással, hogy a virtuális gépek védhetők.
* **Összegyűjtött adatok**: hello szolgáltatás gyűjti, feldolgozza és továbbítja a felügyeleti tanúsítvány adatait a VMM-kiszolgálóról hello kijelölt tooprovide vész-helyreállítási használatával hello szolgáltatás hello VMM kiszolgáló neve, a regisztrálás után és a VMM-kiszolgálón virtuálisgép-felhők hello neve.
* **Az információk felhasználása**:

  * Felügyeleti tanúsítvány – ez használható toohelp azonosításához, és a hitelesítéshez hozzáférési toohello szolgáltatás hello regisztrált VMM-kiszolgálón. hello szolgáltatás hello nyilvános kulcs részét használja hello tanúsítvány toosecure egy jogkivonatot, amely csak hello regisztrált VMM-kiszolgáló is hozzáférhetnek. hello kell kiszolgálót toouse a token toogain hozzáférési toohello szolgáltatás funkciókat.
  * Hello VMM-kiszolgáló nevét – szükséges tooidentify hello VMM-kiszolgáló nevét és hello megfelelő a VMM-kiszolgálóval való kommunikációhoz mely hello felhők találhatók.
  * A felhő neve hello VMM-kiszolgálóról – hello felhő nevének megadása kötelező, az alábbiakban funkció párosítást/párosításának megszüntetéséhez hello szolgáltatás felhő használatakor. Ha úgy dönt, toopair a felhőben az elsődleges adatközpont a másik felhőalapú hello helyreállítási adatközpontban, az összes hello felhő hello helyreállítási adatközpontból hello nevei jelennek meg.
* **Választás**: ezek az információk nem hello szolgáltatás regisztrációs folyamat nagyon fontos részét képezik, mert segít és hello szolgáltatás tooidentify hello VMM-kiszolgáló legyen tooprovide Azure Site Recovery általi védelem, valamint tooidentify hello megfelelő regisztrált VMM-kiszolgálót. Ha ezen információk toohello szolgáltatás toosend nem szeretné, ne használja ezt a szolgáltatást. Ha regisztrálja a kiszolgálót, és később szeretné toounregister, ehhez hello VMM kiszolgálóadatok törlésével hello szolgáltatás portálján (amely az Azure-portálon hello).

**A szolgáltatás: Azure Site Recovery általi védelem engedélyezése**

* **Működés**: hello Azure Site Recovery Provider hello VMM-kiszolgálón telepítve a hello átjáró hello szolgáltatás való kommunikációhoz. hello szolgáltató egy dinamikus csatolású függvénytár (DLL) üzemeltetett hello VMM folyamatban. Hello szolgáltató van telepítve, után hello "Adatközpont-helyreállítási" lekérdezi engedélyezve van a hello VMM felügyeleti konzolján. Felhő minden új vagy meglévő virtuális gépek engedélyezheti "Adatközpont-helyreállítási" nevű tulajdonságot toohelp hello virtuális gép védelmét. Amennyiben ez a tulajdonság értéke, hello szolgáltató küldi hello nevét és Azonosítóját hello virtuális gép toohello szolgáltatás. hello virtuális védelem engedélyezve van a Windows Server 2012 vagy Windows Server 2012 R2 Hyper-V replikációs technológiával. hello virtuális gép adatainak beolvasása replikálódnak az egy Hyper-V gazdagép tooanother (általában a különböző "helyreállítási" adatközpontban található).
* **Összegyűjtött adatok**: hello szolgáltatás gyűjti, folyamatokat, és továbbítja a metaadatok hello virtuális gép esetén hello nevével, az azonosító, a virtuális hálózati és a hello felhő hello nevét, amelyhez tartozik.
* **Az információk felhasználása**: hello szolgáltatást használ hello fent információk toopopulate hello virtuális gép adatait a portálon.
* **Választás**: Ez hello szolgáltatás nagyon fontos részét képezik, és nem kapcsolható ki. Ha nem szeretné ezt az információt küld toohello szolgáltatás, nem engedélyezi az Azure Site Recovery általi védelmet a virtuális gépek. Vegye figyelembe, hogy hello szolgáltató toohello szolgáltatás által küldött minden adathoz HTTPS-KAPCSOLATON keresztül zajlik.

**A szolgáltatás: A helyreállítási terv**

* **Működés**: Ez a szolgáltatás segítségével toobuild vezénylési tervének hello "helyreállítási" adatközpont. Megadhatja, hogy mely hello a virtuális gépek vagy virtuális gépek csoportjának el kell indulniuk hello helyreállítási helyen hello sorrendje. Bármely automatizált parancsfájlokat toobe futtassa, vagy a manuális műveletet toobe készít, helyreállításkor hello az egyes virtuális gépek is megadható. (Hello a következő szakaszban ismertetett) feladatátvételi hello koordinált helyreállítási szintje a helyreállítási terv általában váltja ki.
* **Összegyűjtött adatok**: hello szolgáltatás gyűjti, folyamatokat, és továbbítja a metaadatok hello helyreállítási terv, beleértve a virtuális gép metaadatait, és bármilyen automatizálási parancsfájlokat és a manuális műveletet megjegyzések metaadatait.
* **Az információk felhasználása**: a fent leírt hello metaadatai nem használt toobuild hello helyreállítási tervet a portálon.
* **Választás**: Ez hello szolgáltatás nagyon fontos részét képezik, és nem kapcsolható ki. Ha nem szeretné ezt az információt küld toohello szolgáltatás, nem hozhat létre helyreállítási tervek a szolgáltatásban.

**Szolgáltatás: A hálózatleképezés**

* **Működés**: Ez a funkció lehetővé teszi toomap hálózati adatok hello elsődleges data center toohello helyreállítási adatközpontból. Hello virtuális gépek helyreállítás hello helyreállítási helyen, ha ez a leképezés hozzájárul hálózati kapcsolatot lehessen rájuk vonatkozóan.
* **Összegyűjtött adatok**: hello hálózati leképezési szolgáltatás részeként hello szolgáltatás gyűjti, feldolgozza és továbbítja a hello metaadatok hello logikai hálózatok (elsődleges és datacenter) minden egyes hely esetében.
* **Az információk felhasználása**: hello szolgáltatást használ hello metaadatok toopopulate a portálon ahol leképezheti hello hálózati adatok.
* **Választás**: Ez hello szolgáltatás nagyon fontos részét képezik, és nem kapcsolható ki. Ha nem szeretné, a szolgáltatás toohello küldött információk, ne használjon hello hálózati leképezés funkciója.

**A szolgáltatás: Feladatátvevő - tervezett, nem tervezett, teszt**

* **Működés**: Ez a szolgáltatás segít a virtuális gép egy VMM által felügyelt adatok center tooanother VMM által felügyelt adatközpontból. hello feladatátvételi művelet hello felhasználó számára a portálon váltja ki. Lehetséges feladatátvevő okai például egy nem tervezett esemény (például az természetes disaster0; hello esetben egy tervezett esemény (például datacenter terheléselosztási); a feladatátvételi tesztet (például a helyreállítási terv részletes).

hello szolgáltató hello VMM-kiszolgálón lekérdezi hello esemény hello szolgáltatás értesítést kap, és egy feladatátvételi művelet végrehajtása a hello Hyper-V gazdagép VMM felületeken keresztül. Tényleges feladatátvételt hello virtuális gép egy Hyper-V gazdagép tooanother (általában a különböző "helyreállítási" adatközpont futtató) hello Windows Server 2012 vagy Windows Server 2012 R2 Hyper-V replikációs technológiával kezeli. Hello feladatátvételi befejeződése után hello hello "helyreállítási" adatközpont hello VMM kiszolgálón telepített szolgáltató küldi hello sikeres információk toohello szolgáltatás.

* **Összegyűjtött adatok**: hello szolgáltatást használ hello fent információk toopopulate hello állapotának hello feladatátvételi műveleti adatait a portálon.
* **Az információk felhasználása**: hello szolgáltatást használja hello a fenti adatokat az alábbiak szerint:

  * Felügyeleti tanúsítvány – ez használható toohelp azonosításához, és a hitelesítéshez hozzáférési toohello szolgáltatás hello regisztrált VMM-kiszolgálón. hello szolgáltatás hello nyilvános kulcs részét használja hello tanúsítvány toosecure egy jogkivonatot, amely csak hello regisztrált VMM-kiszolgáló is hozzáférhetnek. hello kell kiszolgálót toouse a token toogain hozzáférési toohello szolgáltatás funkciókat.
  * Hello VMM-kiszolgáló nevét – szükséges tooidentify hello VMM-kiszolgáló nevét és hello megfelelő a VMM-kiszolgálóval való kommunikációhoz mely hello felhők találhatók.
  * A felhő neve hello VMM-kiszolgálóról – hello felhő nevének megadása kötelező, az alábbiakban funkció párosítást/párosításának megszüntetéséhez hello szolgáltatás felhő használatakor. Ha úgy dönt, toopair a felhőben az elsődleges adatközpont a másik felhőalapú hello helyreállítási adatközpontban, az összes hello felhő hello helyreállítási adatközpontból hello nevei jelennek meg.
* **Választás**: Ez hello szolgáltatás nagyon fontos részét képezik, és nem kapcsolható ki. Ha nem szeretné, a szolgáltatás toohello küldött információk, ne használja ezt a szolgáltatást.

## <a name="next-steps"></a>Következő lépések
A teszt feladatátvételi toocheck környezetét a várt módon működik, futtatását követően [megismerése](site-recovery-failover.md) feladatátvételek különböző típusú.
