---
title: "a VMM-felhők tooAzure aaaReplicate Hyper-V virtuális gépek |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooreplicate Hyper-V virtuális gépek Hyper-V-gazdagépek a System Center VMM-felhők tooAzure található."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 9d526a1f-0d8e-46ec-83eb-7ea762271db5
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/23/2017
ms.author: raynew
ms.openlocfilehash: 136f68585534c17b615ecc4e82c0133bf813503f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure"></a>A VMM-felhők tooAzure Hyper-V virtuális gépek replikálása
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Klasszikus portál](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – Klasszikus](site-recovery-deploy-with-powershell.md)
>
>

hello Azure Site Recovery szolgáltatás hozzájárul tooyour az üzletmenet folytonosságának és vész helyreállítási (BCDR) stratégia megvalósításában replikációjának, feladatátvételének és helyreállításának virtuális gépek és fizikai kiszolgálók által. Gépek replikált tooAzure vagy tooa másodlagos helyszíni adatközpontba lehet. A szolgáltatás gyors áttekintéséhez olvassa el az [Azure Site Recovery szolgáltatás mibenlétével foglalkozó](site-recovery-overview.md) cikket.

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan toodeploy Site Recovery tooreplicate Hyper-V virtuális gépek Hyper-v gazdakiszolgálókat, hogy a VMM-magánfelhőkben tooAzure találhatók.

hello cikk hello forgatókönyv előfeltételei tartalmazza, és bemutatja, hogyan tooset be a Site Recovery-tároló segítségnyújtáshoz hello hello forrás VMM-kiszolgálón, a telepített Azure Site Recovery Provider regisztrálja hello kiszolgálót hello tárolóban, Azure-tárfiók hozzáadása, hello telepítése Az Azure Recovery Services Agent ügynököt a Hyper-V gazdakiszolgálókra, adja meg, amely alkalmazott tooall védett virtuális gépek fog, majd engedélyezze a védelmet azon virtuális gépek VMM-felhő védelmi beállításait. Végül tesztelési hello feladatátvételi toomake meg arról, hogy minden a várt módon működik.

Ez a cikk vagy a hello hello alsó megjegyzések vagy kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Architektúra
![Architektúra](./media/site-recovery-vmm-to-azure-classic/topology.png)

* hello Azure Site Recovery Provider telepítve van a VMM hello Site Recovery üzembe helyezése során, és hello VMM-kiszolgáló regisztrálva hello Site Recovery tárolójában. hello szolgáltató kommunikál a Site Recovery toohandle replikálás előkészítését.
* hello Azure Recovery Services Agent ügynököt a Hyper-V gazdakiszolgálókra telepítve van a Site Recovery üzembe helyezése során. Replikációs tooAzure adattárolás kezeli.

## <a name="azure-prerequisites"></a>Azure-előfeltételek
A következőkre lesz szüksége az Azure-ban.

| **Előfeltétel** | **Részletek** |
| --- | --- |
| **Azure-fiók** |Szüksége lesz egy [Microsoft Azure](https://azure.microsoft.com/)-fiókra. Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is. [További információk](https://azure.microsoft.com/pricing/details/site-recovery/) a Site Recovery díjszabásáról. |
| **Azure Storage** |Szüksége lesz egy Azure storage fiók toostore replikált adatokat. A replikált adatokat az Azure Storage tárolja, ha pedig feladatátvétel következik be, a rendszer Azure virtuális gépeket hoz létre. <br/><br/>Ehhez [standard georedundáns tárfiókot kell használnia](../storage/common/storage-redundancy.md#geo-redundant-storage). hello fióknak kell lennie a hello és hello Site Recovery szolgáltatásnak ugyanabban a régióban, és társítható ugyanahhoz az előfizetéshez hello. Vegye figyelembe, hogy a replikációs toopremium tárfiókok jelenleg nem támogatott, és használatuk kerülendő.<br/><br/>[Itt részletesebben olvashat](../storage/common/storage-introduction.md) az Azure Storage-ről. |
| **Azure-hálózat** |Szüksége lesz egy, az Azure virtuális gépek csatlakozni fognak az Azure virtuális hálózat toowhen feladatátvételt hajt végre. hello Azure virtuális hálózat kell hello és hello Site Recovery-tárolónak ugyanabban a régióban. |

## <a name="on-premises-prerequisites"></a>Helyszíni előfeltételek
Az alábbiakra lesz szüksége a helyszínen.

| **Előfeltétel** | **Részletek** |
| --- | --- |
| **VMM** |Szüksége lesz legalább egy önálló fizikai vagy virtuális kiszolgálóként vagy virtuális fürtként üzemelő VMM-kiszolgálóra. <br/><br/>hello VMM-kiszolgálón kell futtatnia a System Center 2012 R2 hello legújabb kumulatív frissítéseket.<br/><br/>Hello VMM-kiszolgálón legalább egy felhőnek kell.<br/><br/>hello tooprotect kívánt forrásfelhőnek legalább egy VMM-gazdagépcsoportot kell tartalmaznia.<br/><br/>A VMM-felhőkről a Keith Mayer blogjában olvasható [Walkthrough: Creating private clouds with System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) (Bemutató: magánfelhők létrehozása a System Center 2012 SP1 VMM-mel) című cikkből tudhat meg továbbiakat. |
| **Hyper-V** |Egy vagy több Hyper-V gazdakiszolgálók vagy fürtök a VMM-felhő hello lesz szüksége. hello gazdakiszolgálókon és egy vagy több virtuális gépeket. <br/><br/>hello Hyper-V kiszolgáló kell futnia, legalább **Windows Server 2012 R2** hello Hyper-V szerepkörrel vagy **Microsoft Hyper-V Server 2012 R2** és a legújabb frissítések telepítve rendelkezik hello.<br/><br/>A virtuális gépeket tartalmazó Hyper-V server kívánt tooprotect egy VMM-felhőben kell lennie.<br/><br/>Ha fürtben futtatja a Hyper-V-t, ne feledje, hogy ha statikus IP-címen alapuló fürtöt használ, a rendszer nem hozza létre automatikusan a fürtszervezőt. Manuálisan kell tooconfigure hello fürtszervező. [További információkat](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) Aidan Finn blogbejegyzésében talál. |
| **Védett gépek** | Virtuális gépek kívánt tooprotect meg kell felelnie [Azure-követelményeknek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). |

## <a name="network-mapping-prerequisites"></a>Hálózatleképezési előfeltételek
Ha virtuális gépet az Azure-hálózat hozzárendelés véd felel meg a Virtuálisgép-hálózatok hello forrás VMM-kiszolgálón és a cél Azure-hálózatok tooenable hello következő között:

* Feladatok átadása a hello azonos gépeire hálózati kapcsolódhatnak tooeach más, függetlenül a helyreállítási terv.
* Ha hello cél Azure-hálózatban hálózati átjáró be van állítva, a virtuális gépek kapcsolódhatnak tooother a helyszíni virtuális gépek.
* Ha nem adja meg a hálózati leképezése csak virtuális gépek feladatátvételt hello azonos helyreállítási terv feladatátvételi tooAzure után lesznek képesek tooconnect tooeach más.

Ha azt szeretné, hogy toodeploy hálózatra való leképezés, hello következőkre lesz szüksége:

* azt szeretné, hogy a forrás VMM-kiszolgálón hello tooprotect hello virtuális gépek csatlakoztatott tooa Virtuálisgép-hálózathoz kell lennie. Erre a hálózatra kell csatolt tooa hello felhő társított logikai hálózat.
* Egy Azure-hálózat toowhich replikált virtuális gépek a feladatátvételt követően csatlakozni tudnak. Ezt a hálózatot a feladatátvételi hello időpontjában válasszon ki. hello hello hálózat legyen ugyanabban a régióban, az Azure Site Recovery-előfizetést.


Hálózatok előkészítése a VMM-ben:

   * [Állítson be logikai hálózatokat](https://technet.microsoft.com/library/jj721568.aspx).
   * [Állítson be virtuálisgép-hálózatokat](https://technet.microsoft.com/library/jj721575.aspx).


## <a name="step-1-create-a-site-recovery-vault"></a>1. lépés: Site Recovery-tároló létrehozása
1. Jelentkezzen be toohello [kezelési portál](https://portal.azure.com) tooregister kívánt hello VMM-kiszolgálóról.
2. Kattintson a **Data Services** > **Recovery Services** > **Site Recovery-tároló** lehetőségre.
3. Kattintson az **Új létrehozása** > **Gyorslétrehozás** elemre.
4. A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban.
5. A **régió**, válassza ki a hello hello tároló földrajzi régióját. támogatott toocheck régiók, tekintse meg a cikknek a földrajzi elérhetőséggel [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Kattintson a **Tároló létrehozása** elemre.

    ![Új tároló](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Ellenőrizze, hogy hello állapot sáv tooconfirm hello tároló sikeresen létrejött. hello tároló tulajdonosaként **aktív** a hello Recovery Services főoldalán.

## <a name="step-2-generate-a-vault-registration-key"></a>2. lépés: A tároló regisztrációs kulcsának létrehozása
Hozzon létre egy regisztrációs kulcsot hello tárolóban lévő állapottal. Miután hello Azure Site Recovery Provider letöltése, és telepítse a VMM-kiszolgálón hello, a kulcs tooregister hello VMM-kiszolgáló hello tárolóban fogja használni.

1. A hello **Recovery Services** hello tároló tooopen hello gyors kezdés lapon kattintson. Gyors üzembe helyezési hello ikon segítségével is megnyithatja.

    ![Első lépések ikon](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)
2. Hello legördülő listában válassza ki a **egy helyszíni VMM-hely és a Microsoft Azure között**.
3. A **Prepare VMM Servers** (VMM-kiszolgálók előkészítése) résznél kattintson a **Generate registration key file** (Regisztrációskulcs-fájl generálása) lehetőségre. hello kulcsfájl automatikusan létrejön-e, és ezt követően 5 napig érvényes. Ha nem éri el az Azure portál hello hello VMM-kiszolgálóról szüksége lesz toocopy toohello fájlkiszolgálót.

    ![Regisztrációs kulcs](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-hello-azure-site-recovery-provider"></a>3. lépés: Hello Azure Site Recovery Provider telepítése
1. A **gyors üzembe helyezés** > **előkészítése a VMM-kiszolgálók**, kattintson a **töltse le a Microsoft Azure Site Recovery Provider telepítése a VMM Server** tooobtain hello hello Provider telepítőfájlja legújabb verzióját.
2. Futtassa ezt a fájlt hello forrás VMM-kiszolgálón.

   > [!NOTE]
   > Ha a VMM telepítve van a fürtben, és telepítése hello szolgáltató hello az első alkalommal telepítse azt aktív csomópontra, és Befejezés hello telepítési tooregister hello VMM-kiszolgáló hello tárolóban lévő állapottal. Majd telepítse hello szolgáltató hello más csomópontokra. Vegye figyelembe, hogy ha frissít, akkor szüksége lesz tooupgrade az összes olyan csomóponton, mert azok kell összes szolgáltató hello futó szolgáltató azonos verziója hello.
   >
   >
3. Telepítő hello nem egy előfeltételeket, majd ellenőrizze, és kér engedélyt toostop hello a VMM szolgáltatás toobegin szolgáltató beállítása. a VMM szolgáltatás hello automatikusan újraindul a telepítő befejezése után. A telepítése a VMM-fürthöz lesz kéri toostop hello fürtszerepkört.
4. A **Microsoft Update** lapon kérheti a frissítések telepítését. Ez a beállítás engedélyezett frissítéseket telepíti tooyour Microsoft Update-szabályzatnak megfelelően.

    ![Microsoft Update](./media/site-recovery-vmm-to-azure-classic/updates.png)
5. telepítési helye a következő szolgáltató értéke túl hello hello**<SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin**. Kattintson az **Install** (Telepítés) gombra.

   ![Telepítési hely](./media/site-recovery-vmm-to-azure-classic/install-location.png)
6. Szolgáltató van telepítve hello után kattintson a **regisztrálása** tooregister hello server hello tárolóban lévő állapottal.

    ![Telepítés kész](./media/site-recovery-vmm-to-azure-classic/install-complete.png)
7. A **tároló neve**, ellenőrizze, melyik hello a kiszolgálót regisztrálni fogja hello tároló hello nevére. Kattintson a *Tovább* gombra.

    ![Kiszolgáló regisztrációja](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)
8. A **internetkapcsolat** adjon meg hello VMM futó Provider hogyan hello server toohello Internet csatlakozik. Válassza ki **csatlakozás meglévő proxybeállításokkal** toouse hello alapértelmezett internetkapcsolati beállításait hello kiszolgálón konfigurált.

    ![Internetbeállítások](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

   * Ha azt szeretné, hogy toouse egyéni proxyt érdemes beállítania, hello szolgáltató telepítése előtt. Ha egyéni proxyt a beállításokat egy teszt lefuttatásával toocheck hello proxykapcsolatot.
   * Ha egyéni proxyt használ, vagy az alapértelmezett proxy hitelesítést igényel tooenter hello proxy adatait, beleértve a hello proxy címét és portját lesz szüksége.
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
13. Kattintson a **következő** toocomplete hello folyamat. A regisztrációt követően az Azure Site Recovery által lekéri metaadatait hello VMM-kiszolgálóról. hello kiszolgáló megjelenik a hello **VMM-kiszolgálókon** hello lapján **kiszolgálók** lap hello tárolóban lévő állapottal.

    ![Utolsó oldal](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

A regisztrációt követően az Azure Site Recovery által lekéri metaadatait hello VMM-kiszolgálóról. hello kiszolgáló megjelenik a hello **VMM-kiszolgálókon** lapon látható hello **kiszolgálók** lap hello tárolóban lévő állapottal.

### <a name="command-line-installation"></a>Parancssori telepítés
hello Azure Site Recovery Provider a következő parancssori hello használatával is telepíthető. Ez a módszer egy Server Core a Windows Server 2012 R2 használt tooinstall hello szolgáltató is lehet.

1. Töltse le a hello szolgáltató telepítési és a regisztrációs kulcs tooa mappát. A mappa lehet például a következő: C:\ASR.
2. Hello System Center Virtual Machine Manager szolgáltatás leállítása
3. Egy rendszergazda jogú parancssorból nyerje ki a hello Provider telepítőjét a következő parancsokkal:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Telepítse a hello szolgáltatót az alábbiak szerint:

        C:\ASR> setupdr.exe /i
5. Regisztrálja a hello szolgáltatót az alábbiak szerint:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>       

A paraméterek a következőt jelentik:

* **/ Credentials** : kötelező paraméter, amely hello helyet, mely hello a regisztrációs kulcs fájlját található  
* **/ FriendlyName** : hello Hyper-V gazdakiszolgáló hello Azure Site Recovery portálon megjelenő nevét hello paramétert kötelező megadni.
* **/ EncryptionEnabled** : toospecify nem kötelező paraméter, ha a virtuális gépek Azure-ban (inaktív adatok titkosítása) a tooencryption szeretné. hello nevének kell lennie egy **.pfx** bővítmény.
* **/ proxyaddress** : nem kötelező paraméter, amely hello hello proxykiszolgáló címét.
* **/ proxyport** : nem kötelező paraméter, amely hello proxykiszolgáló hello port.
* **/ proxyusername** : nem kötelező paraméter, amely hello proxy felhasználónevét határozza meg.
* **/ proxypassword** : nem kötelező paraméter, amely hello proxy jelszavát.  

## <a name="step-4-create-an-azure-storage-account"></a>4. lépés: Azure-tárfiók létrehozása
1. Ha nem rendelkezik Azure-tárfiók kattintson **egy Azure Storage-fiók hozzáadása** toocreate fiókkal.
2. A fióknál engedélyezze a georeplikációt. Kell a hello és hello Azure Site Recovery szolgáltatásnak ugyanabban a régióban, és hello társítható ugyanahhoz az előfizetéshez.

    ![Tárfiók](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [!NOTE]
> [A tárfiókok áttelepítési](../azure-resource-manager/resource-group-move-resources.md) erőforrás csoportok belül hello ugyanahhoz az előfizetéshez vagy előfizetések között nem támogatott a Site Recovery telepítéséhez használt tárfiókok.
>
>

## <a name="step-5-install-hello-azure-recovery-services-agent"></a>5. lépés: Hello Azure Recovery Services Agent telepítése
Hello Azure Recovery Services agent telepítése a VMM-felhő hello minden Hyper-V gazdagép-kiszolgálón.

1. Kattintson a **gyors üzembe helyezés** > **Azure Site Recovery Services Agent letöltése és telepítése a gazdagépeken** tooobtain hello legújabb hello ügynök telepítési fájl verziója.

    ![Recovery Services Agent telepítése](./media/site-recovery-vmm-to-azure-classic/install-agent.png)
2. Futtassa a hello telepítőfájlt minden Hyper-V gazdagép-kiszolgálón.
3. A hello **szükséges előfeltételek ellenőrzése** kattintson **következő**. A rendszer automatikusan telepíti a hiányzó előfeltételeket.

    ![Recovery Services Agent, előfeltételek](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)
4. A hello **telepítési beállítások** lapján adja meg a tooinstall hello ügynök és a biztonsági mentés metaadatainak telepítve válassza hello gyorsítótár helyét. Ezt követően kattintson a **Telepítés** gombra.
5. Kattintson a telepítés végeztével **Bezárás** toocomplete hello varázsló.

    ![A MARS Agent regisztrálása](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Parancssori telepítés
Microsoft Azure Recovery Services Agent hello ezzel a paranccsal hello parancssorból is telepíthető:

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>6. lépés: A felhő védelmi beállításainak konfigurálása
Hello VMM-kiszolgáló regisztrálása után konfigurálhatja a felhő védelmi beállításait. Engedélyezte a hello beállítás **Synchronize cloud data hello tárolóban** hello VMM-kiszolgálón futó összes felhő hello fog megjelenni, ha telepítve hello szolgáltató <b>védett elemek</b> lapon hello tárolóban lévő állapottal.

![Közzétett felhő](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. Hello gyors kezdés lapon kattintson **védelem beállítása a VMM-felhőkben**.
2. A hello **védett elemek** lapra, majd hello felhő tooconfigure szeretne, és nyissa meg toohello **konfigurációs** fülre.
3. A **Target** (Cél) beállításnál válassza az **Azure** lehetőséget.
4. A **Tárfiók** válassza ki a replikáláshoz használni kívánt hello Azure storage-fiók.
5. Állítsa be **tárolt adatok titkosítása** túl**ki**. A beállítással megadható, hogy adatokat titkosítani kell hello helyszíni hely és az Azure közötti replikáció során.
6. A **másolás gyakorisága** hagyja hello alapértelmezett értéket. Ez az érték határozza meg, hogy a rendszer milyen gyakran szinkronizálja az adatokat a forrás- és a célhelyek között.
7. A **helyreállítási pontok megőrzése**, hagyja hello alapértelmezett értéket. Alapértelmezett értéke nulla lenne csak hello legfrissebb helyreállítási pontot az elsődleges virtuális gép tárolja a replika gazdakiszolgálón.
8. A **alkalmazáskonzisztens pillanatképek gyakorisága**, hagyja hello alapértelmezett értéket. Ez az érték határozza meg, milyen gyakran toocreate pillanatképeket. Pillanatképek a kötet árnyékmásolata szolgáltatás (VSS) tooensure, amely az alkalmazások konzisztens állapotban legyenek, hello pillanatkép készítésének időpontjában.  Ha egy érték, győződjön meg arról, hogy nem éri el hello további helyreállítási pontok száma.
9. A **replikáció kezdési ideje**, adja meg a kezdeti replikációja, adatok tooAzure kezdetének. hello időzóna hello Hyper-V gazdagép-kiszolgálón használható. Azt javasoljuk, hogy úgy ütemezze a kezdeti replikáció hello csúcsidőn.

    ![Felhőreplikáció beállításai](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

Hello beállítások mentését követően a feladat jön létre, és figyelni az hello **feladatok** fülre. A replikáció hello VMM-forrásfelhőben található összes Hyper-V gazdakiszolgálók lesz konfigurálva.

A mentés után felhőbeállítások módosíthatók hello **konfigurálása** külön-külön toomodify hello cél helyét, vagy a cél tárfiók lesz tooremove hello felhőkonfiguráció kell, és konfigurálja újra a hello felhő. Figyelje meg, hogy hello tárolási fiókját hello módosítása csak akkor érvényes, amely után hello tárfiók módosult védelemre engedélyezett virtuális gépekhez. Meglévő virtuális gépek nem települnek át toohello új tárfiókot.

## <a name="step-7-configure-network-mapping"></a>7. lépés: Hálózatleképezés konfigurálása
Mielőtt elkezdené hálózatleképezés ellenőrizze, hogy hello forrás VMM-kiszolgálón található virtuális gépek csatlakoztatott tooa Virtuálisgép-hálózatot. Ezenfelül hozzon létre legalább egy Azure virtuális hálózatot. Vegye figyelembe, hogy több Virtuálisgép-hálózatot is csatlakoztatott tooa egyetlen Azure-hálózatra.

1. Hello gyors kezdés lapon kattintson **hálózatok leképezése**.
2. A hello **hálózatok** lap **adatforrásról**, jelölje be hello forrás VMM-kiszolgálón. A **Target location** (Cél helye) résznél válassza az Azure lehetőséget.
3. A **forrás** hálózatok hello VMM-kiszolgálóhoz tartozó Virtuálisgép-hálózatok listája jelenik meg. A **cél** hálózatok hello Azure hello előfizetéshez társított hálózatok jelennek meg.
4. Válasszon hello forrás Virtuálisgép-hálózatot, és kattintson a **térkép**.
5. A hello **válasszon egy Célhálózatot** lapon, jelölje be hello cél toouse kívánt Azure-hálózat.
6. Kattintson a hello pipa toocomplete hello megfeleltetési folyamat.

    ![Felhőreplikáció beállításai](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

Hello beállítások mentését követően elindul egy feladat tootrack hello leképezési művelet előrehaladását, és figyelhető hello feladatok lapján. Minden meglévő replika virtuális gépek toohello forrás Virtuálisgép-hálózatnak megfelelő lesznek csatlakoztatott toohello cél Azure-hálózatok. Új virtuális gépek, amelyek a forrás Virtuálisgép-hálózathoz csatlakoztatott toohello replikáció után lesz csatlakoztatott toohello leképezett Azure-hálózathoz. Ha módosít egy meglévő hozzárendelést egy új hálózati, a replika virtuális gépek csatlakoznak hello új beállításokkal.

Vegye figyelembe, hogy ha hello célhálózatban már több alhálózat működik, és ezek egyikének rendelkezik hello neve megegyezik a virtual machine hello forrás alhálózati található, akkor hello replika virtuális gép lesz a cél alhálózathoz csatlakoztatott toothat feladatátvételt követően. Ha nincsenek egyező nevű cél alhálózathoz, hello virtuális gép lesz a csatlakoztatott toohello hello hálózat első alhálózatát.

> [!NOTE]
> [Áttelepítési hálózatok](../azure-resource-manager/resource-group-move-resources.md) erőforrás csoportok belül ugyanahhoz az előfizetéshez hello vagy előfizetések között nem támogatott a Site Recovery üzembe helyezéséhez használt hálózatok.
>
>

## <a name="step-8-enable-protection-for-virtual-machines"></a>8. lépés: A virtuális gépek védelmének engedélyezése
Kiszolgálók, felhők és hálózatok megfelelő konfigurálását követően engedélyezheti a hello felhő virtuális gépek védelmét. Vegye figyelembe a következőket hello:

* A virtuális gépeknek meg kell felelniük az [Azure-követelményeknek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
* hello virtuális gép tooenable védelmi hello operációsrendszer- és operációsrendszer-lemez tulajdonságait kell állítani. Ha egy virtuális gép létrehozása a VMM-virtuálisgép-sablon használatával beállíthatja hello tulajdonság. Ezeket a tulajdonságokat a meglévő virtuális gépek állítsa be a hello **általános** és **hardverkonfiguráció** hello virtuális gép tulajdonságok lapján. Ha ezeket a tulajdonságokat a VMM-ben nem állít be fog tudni tooconfigure őket hello Azure Site Recovery portálon.

    ![Virtuális gép létrehozása](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![A virtuális gépek tulajdonságainak módosítása](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)

1. tooenable védelmet, a hello **virtuális gépek** mely hello a virtuális gép is található hello felhőben lapra, majd **engedélyezni a védelmet** > **virtuálisgépekhozzáadása**.
2. A virtuális gépek hello felhőben hello listájából válassza ki azt szeretné, hogy tooprotect egy hello.

    ![A virtuális gép védelmének engedélyezése](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    Hello előrehaladását úgy követheti nyomon **Védelemengedélyezési** hello műveletét **feladatok** lapon, beleértve a kezdeti replikáció hello. Hello után **Védelemvéglegesítési** feladat futtatása hello virtuális gép készen áll a feladatátvételre. Miután a védelem engedélyezve van, és a virtuális gép replikálását, képes tooview lesz őket az Azure-ban.

    ![A virtuális gép védelme feladat](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

1. Ellenőrizze a hello virtuálisgép-tulajdonságokat, és szükség szerint módosítsa.

    ![Virtuális gépek ellenőrzése](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)
2. A hello **konfigurálása** hello virtuális gép tulajdonságok következő hálózati tulajdonságok lapon módosítható.

* **Hello cél virtuális gép hálózati adapterek számát** -hello hálózati adapterek számát határozza meg hello mérete hello cél virtuális gép határozza meg. Ellenőrizze [virtuális gép mérete specifikációk](../virtual-machines/linux/sizes.md) hello hello virtuális gép mérete által támogatott adapterek számát. Módosítsa a virtuális gép hello méretét, és hello beállítások mentéséhez, hálózati adapterek száma hello módosítják hello hello következő megnyitásakor **konfigurálása** lap. a cél virtuális gépek hálózati adapterek hello száma: hello hálózati adaptereken a forrás virtuális gép és hello maximális száma hello választása esetén az alábbiak szerint hello virtuális gép mérete által támogatott minimális száma:

  * Ha hálózati adapterek hello forrásgépen hello száma kisebb vagy egyenlő, mint hello cél gép méretéhez engedélyezett adapterek toohello számát, majd hello cél lesz hello ugyanannyi adapter hello forrásaként.
  * Ha hello hello forrás virtuális gép adaptereinek száma meghaladja hello engedélyezett hello célméretet majd hello cél maximális fogja használni.
  * Például ha a forrásgépen két hálózati adaptert, és hello célgép mérete négy támogatja, akkor hello célgépen két adapter fog működni. Ha hello forrásgépen két adapter, azonban a hello támogatott célméretet csak egy majd hello célgépen csak egy adapter fog működni.     
* **Hello cél virtuális gép hálózati** -hello hálózati toowhich hello virtuálisgép kapcsolódik toois hálózatra való leképezést a forrás virtuális gép hello hálózat határozza meg. Hello forrás virtuális gépen egynél több hálózati adapter és a forrás hálózatok esetén csatlakoztatott toodifferent célhálózatokhoz, akkor telepítenie kell toochoose közötti hello célhálózatok közül.
* **Egyes hálózati adapterek alhálózati** – az összes hálózati adapteren, kiválaszthatja a hello alhálózati toowhich hello átadja a virtuális gép csatlakozni fog.
* **Cél IP-címe** – Ha a forrás virtuális gép hello hálózati adapter-e egy statikus IP-címet, akkor megadhatja hello IP-cím hello cél virtuális gép konfigurált toouse. Ezzel a szolgáltatással megőrizni a forrás virtuális gép hello IP-címét egy feladatátvétel után. Ha IP-cím áll rendelkezésre majd bármilyen elérhető IP-címet kap toohello hálózati adapter feladatátvételi hello időpontban. Ha hello cél IP-cím van megadva, de már használja egy másik virtuális gép Azure-beli feladatátvétel sikertelen lesz.  

    ![Hálózati tulajdonságok módosítása](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

> [!NOTE]
> A statikus IP-címet használó linuxos virtuális gépeket a rendszer nem támogatja.
>
>

## <a name="test-hello-deployment"></a>Hello központi telepítés tesztelése
Tervezze meg a központi telepítés futtathatja egyetlen virtuális gép feladatátvételi tesztjét, vagy hozzon létre egy helyreállítási terv álló több virtuális gép, és a hello feladatátvételi teszt futtatása tootest.  

A feladatátvételi teszt segítségével elkülönített hálózatban próbálhatja ki a feladatátvételi és helyreállítási mechanizmusokat. Vegye figyelembe:

* Ha tooconnect toohello virtuális gépet az Azure-ban a távoli asztal hello feladatátvétel után, engedélyezze a távoli asztali kapcsolat hello virtuális gépen, hello a feladatátvételi teszt futtatása előtt.
* Feladatátvétel után a nyilvános IP cím tooconnect toohello távoli asztal használata Azure virtuális gép használja. Ha meg akarja toodo, győződjön meg arról, nincs érvényben tartományi szabályzatok, amelyek meggátolják, kapcsolódó tooa virtuális gép nyilvános cím segítségével.

> [!NOTE]
> tooget hello legjobb teljesítményt biztosít, a rendszer átadja tooAzure, győződjön meg arról, hogy telepítette a virtuális gép hello Azure ügynök hello. Ez felgyorsítja a rendszerindítást, és segít a hibák elhárításában. Töltse le a hello [Linux-ügynök](https://github.com/Azure/WALinuxAgent), vagy hello [Windows-ügynök](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

### <a name="create-a-recovery-plan"></a>Helyreállítási terv létrehozása
1. A hello **helyreállítási tervek** lapon maradva adja hozzá egy új tervet. Adjon meg egy nevet, **VMM** a **adatforrástípust**, és hello forrás VMM-kiszolgálót a **forrás**. hello cél Azure lesz.

    ![Helyreállítási terv létrehozása](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)
2. A hello **virtuális gépek kijelölése** lapra, jelölje be a virtuális gépek tooadd toohello helyreállítási terv. A virtuális gépeket adnak toohello helyreállítási terv alapértelmezett csoport – csoport 1. A tesztek során egy helyreállítási tervben legfeljebb 100 virtuális gépet használtunk.

* Tooverify hello virtuális gép tulajdonságainak toohello terv felvétel előtt, kattintson a hello virtuális gép tulajdonságok lapján hello hello felhő gépre. Beállíthatja úgy is hello virtuális gép tulajdonságainak hello VMM-konzolon.
* Az összes megjelenített hello virtuális gépek engedélyezve van a védelem. a listán hello mindkét virtuális gépek, amelyeknél engedélyezve van a védelem és a kezdeti replikáció befejeződött, és azokat, amelyek engedélyezve vannak-védelem és kezdeti replikáció függőben van. Helyreállítási terv részeként csak azoknak a virtuális gépeknek a feladatátvételét lehet végrehajtani, amelyeknek már végbement a kezdeti replikációja.

    ![Helyreállítási terv létrehozása](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

A helyreállítási terv a létrehozásukat követően megjelennek a hello **helyreállítási tervek** fülre. Azt is megteheti [Azure automation-forgatókönyveket](site-recovery-runbook-automation.md) toohello helyreállítási terv tooautomate műveletek során.

### <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása
Nincsenek két módon toorun a teszt feladatátvételi tooAzure.

* **Azure-hálózat nélkül feladatátvételi teszt**– Ez a típus ellenőrzi, hogy hello virtuális megfelelően feláll az Azure-ban. hello virtuális gép nem lesz csatlakoztatott tooany Azure-hálózatot a feladatátvétel után.
* **Az Azure-hálózatot a feladatátvételi teszt**– az ilyen típusú feladatátvételi ellenőrzi, hogy a egész replikációs környezet hello megfelelően feláll-e, és, amely hello virtuális gépek a feladatátvételt csatlakoztatott toohello megadott cél Azure-hálózat. Alhálózati kezelésére, a feladatátvételi teszthez hello alhálózati hello teszt virtuális gép lesz kell szerepelnek hello alhálózat hello replika virtuális gép alapján. Ez az különböző tooregular replikációs, ha egy replika virtuális gép hello alhálózati hello alhálózati hello forrás virtuális gép alapján.

Ha azt szeretné, toorun feladatátvételi teszt Azure-hálózat megadása nélkül védelmi tooAzure engedélyezett virtuális gépek nem kell tooprepare semmit. a cél Azure-hálózat toocreate elkülönített új Azure-beli hálózat az Azure éles hálózattól (alapértelmezett működés, ha az Azure-ban hozzon létre egy új hálózatot) szüksége lesz egy feladatátvételi teszt toorun. Olvassa túl[feladatátvételi teszt futtatása](site-recovery-failover.md) további részleteket.

Is szüksége lesz mentése hello infrastruktúra tooset hello replikált virtuális gép toowork várt módon. Például a tartományvezérlő és DNS-virtuális gép Azure Site Recovery segítségével replikált tooAzure lehet, és hello teszteléshez használt hálózaton a feladatátvételi teszt hozhatók létre. További információkért olvassa el az [Active Directoryra vonatkozó feladatátvételi szempontokat részletező cikket](site-recovery-active-directory.md#test-failover-considerations).

feladatátvételi teszt toorun hello a következő:

1. A hello **helyreállítási tervek** lapra, válassza ki a hello tervet, és kattintson **feladatátvételi teszt**.
2. A hello **erősítse meg a feladatátvételi teszt** lapon jelölje be **nincs** vagy egy adott Azure-hálózatot.  Fontos megjegyezni, hogy ha nincs hello feladatátvételi teszthez lesz, ellenőrizze, hogy a virtuális gép hello replikált megfelelően tooAzure nem ellenőrzi a replikációs hálózat konfigurációját.

    ![Nincs hálózat](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)
3. Ha engedélyezett az adattitkosítás a hello felhőszolgáltatásokat, a **titkosítási kulcs** válassza hello korábban kiadott tanúsítványt hello szolgáltató hello VMM-kiszolgálón, a telepítés során hello beállítás tooenable az adattitkosítást a felhő bekapcsolása .
4. A hello **feladatok** lapon nyomon követheti a feladatátvétel előrehaladását. Meg kell tudni toosee hello virtuális gép tesztelési replikájának hello Azure-portálon. Ha elkészült a beállítással tooaccess virtuális gépek a helyszíni hálózaton is kezdeményezhető a távoli asztali kapcsolat toohello virtuális gépek.
5. Amikor hello feladatátvételi eléri hello **teszt befejezése** fázist, kattintson a **a teljes teszt** toofinish azt. Toohello részletezve **feladat** lapon tootrack feladatátvételi folyamat előrehaladásának és állapotának és tooperform szükséges műveleteket.
6. A feladatátvétel után hello virtuális gép tesztelési replikájának hello Azure-portálon tekintheti meg. Ha elkészült a beállítással tooaccess virtuális gépek a helyszíni hálózaton is kezdeményezhető a távoli asztali kapcsolat toohello virtuális gépek. A következő hello:

   1. Győződjön meg arról, hogy hello virtuális gép sikeresen elindul-e.
   2. Ha tooconnect toohello virtuális gépet az Azure-ban a távoli asztal hello feladatátvétel után, engedélyezze a távoli asztali kapcsolat hello virtuális gépen, hello a feladatátvételi teszt futtatása előtt. Szükség tooadd egy RDP-végpontot hello virtuális gépen. Kihasználhatja a [Azure Automation-forgatókönyveket](site-recovery-runbook-automation.md) toodo, amely.
   3. A feladatátvétel után a nyilvános IP cím tooconnect toohello virtuális gépek használatakor a távoli asztali kapcsolat segítségével győződjön meg arról nincs érvényben tartományi szabályzatok, amelyek meggátolják tooa virtuális gép nyilvános cím segítségével csatlakozzon.
7. Hello a tesztelés után hajtsa végre a következő hello:

   * Kattintson a **hello a feladatátvételi teszt befejeződött**. Hello karbantartása környezet tooautomatically power tesztelése ki, és törölje a hello teszt virtuális gépek.
   * Kattintson a **megjegyzések** toorecord hello feladatátvételi teszthez kapcsolatos megfigyelések feljegyzéséhez mentse.


## <a name="next-steps"></a>Következő lépések
Tájékozódjon a [helyreállítási tervek beállításáról](site-recovery-create-recovery-plans.md) és a [feladatátvételről](site-recovery-failover.md).
