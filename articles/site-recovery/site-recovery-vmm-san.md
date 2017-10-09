---
title: "Azure Site Recovery segítségével a Hyper-V virtuális gépek a VMM-ben SAN aaaReplicate |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooreplicate Hyper-V virtuális gépek az Azure Site Recovery SAN-replikálás használatával két hely között."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: eb480459-04d0-4c57-96c6-9b0829e96d65
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: cee4ff519a8e9e7f29e17e923a9533fb339634b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-vms-in-vmm-clouds-tooa-secondary-site-with-azure-site-recovery-by-using-san"></a>VMM felhők tooa másodlagos hely az Azure Site Recovery szolgáltatással a Hyper-V virtuális gépek replikálása SAN használatával


Ez a cikk használja, ha azt szeretné, hogy toodeploy [Azure Site Recovery](site-recovery-overview.md) Hyper-V virtuális gépek (a System Center Virtual Machine Manager-felhőkben felügyelt) tooa VMM másodlagos hely, a klasszikus portálon hello Azure Site Recovery segítségével toomanage replikációját. Ebben a forgatókönyvben nem érhető el hello új Azure-portálon.



Ez a cikk végén hello megjegyzések utáni. A hello tootechnical kérdésre választ kaphat [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="why-replicate-with-san-and-site-recovery"></a>Miért replikálhat SAN és a Site Recovery?

* SAN egy vállalati szintű, méretezhető replikációs megoldás biztosítja, úgy, hogy egy elsődleges helyet tartalmazó Hyper-V-San hálózathoz képes replikálni, amely logikai egységek tooa másodlagos hely SAN. Tárolási a VMM kezeli, és replikációs és feladatátvételi van összehangolva Site Recovery szolgáltatással.
* A Site Recovery számos együttműködik [SAN tároló partnerek](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) tooprovide replikáció Fibre Channel- és iSCSI-tároló keresztül.  
* A meglévő SAN infrastruktúra tooprotect kritikus fontosságú alkalmazások használata a Hyper-V fürtök rendszerbe. Virtuális gépek csoportként replikálható, így N szintű alkalmazásokhoz következetesen feladatátvételre.
* SAN-replikálás replikációs konzisztencia biztosítja az alkalmazások alacsony RTO és a helyreállítási Időkorlát szinkronizált replikáció különböző rétegek között, és aszinkron módjára való átkapcsolás replikációs (attól függően, hogy a tárolótömb lehetőségeit) nagy rugalmasságot biztosít.  
* A VMM háló rendszerű hello SAN tárterület kezelése, és használjon SMI-S VMM toodiscover meglévő tárolóhoz.  

Vegye figyelembe:
- SAN-replikációt pont-pont nem érhető el hello Azure-portálon. Csak akkor érhető hello a klasszikus portálon. Nem hozható létre új tárolók hello klasszikus portálon. Meglévő tárolók tarthatjuk fenn.
- A SAN tooAzure replikáció nem támogatott.
- Megosztott vhdx-fájlokat nem replikálja, vagy logikai egységek (LUN), amelyek közvetlenül csatlakoztatott tooVMs iSCSI vagy Fibre Channel használatával. Vendégfürtök replikálható.


## <a name="architecture"></a>Architektúra

![SAN-architektúra](./media/site-recovery-vmm-san/architecture.png)

- **Azure**: hello Azure-portálon a Site Recovery-tároló beállítása.
- **SAN-tároló**: a VMM háló rendszerű hello kezelt SAN-tárolás. Adja hozzá a tárolószolgáltatót hello, tárhelybesorolások létrehozása és beállítása a tárolókészletek.
- **A VMM és a Hyper-V**: azt javasoljuk, hogy a VMM-kiszolgáló minden helyen. A VMM magánfelhőket beállítani, és azokat Hyper-V fürtök naplózza. A telepítés során hello Azure Site Recovery Provider telepítve van minden VMM-kiszolgálót, és hello kiszolgáló hello tárolóban regisztrált. hello szolgáltató hello Site Recovery szolgáltatás toomanage replikáció, feladatátvétel és feladat-visszavétel kommunikál.
- **Replikációs**: a VMM-tárolás beállítása és konfigurálása a replikáció a Site Recovery szolgáltatásban, után a replikáció a hello elsődleges és másodlagos SAN-tároló között. Replikációs adatküldést tooSite helyreállítási.
- **Feladatátvételi**: a feladatátvétel hello Site Recovery portálon. Nincs adatvesztés elkerülése feladatátvétel során, mert még szinkron replikálása.
- **Feladat-visszavétel**: toofail ismét engedélyezi a visszirányú replikálás a változási különbözeteket tootransfer hello másodlagos hely toohello elsődleges helyről. A fordított replikáció befejezése után futtatja egy tervezett feladatátvételt a másodlagos tooprimary. A tervezett feladatátvétel leállítja a hello replika virtuális gépek hello másodlagos helyen, és elindítja azokat hello elsődleges helyen.


## <a name="before-you-start"></a>Előkészületek


**Előfeltételek** | **Részletek**
--- | ---
**Azure** |Szüksége van egy [Microsoft Azure](https://azure.microsoft.com/)-fiókra. Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is. [További információk](https://azure.microsoft.com/pricing/details/site-recovery/) a Site Recovery díjszabásáról. Egy Azure Site Recovery-tárolóban tooconfigure létrehozása és kezelése a replikációt és feladatátvételt.
**VMM** | Egyetlen VMM-kiszolgálót használja, és különböző felhők között replikálja, de azt javasoljuk, hogy egy VMM-ben a hello elsődleges hely és egy-egy hello másodlagos helyen. A VMM telepíthető önálló fizikai vagy virtuális kiszolgálóként vagy egy fürtöt. <br/><br/>VMM-kiszolgáló hello futnia kell a System Center 2012 R2 vagy újabb rendszert hello legújabb összesített frissítésekkel.<br/><br/> Legalább egy felhőnek kell VMM-kiszolgálón hello elsődleges tooprotect és hello másodlagos VMM-kiszolgáló egy felhőnek feladatátvételi kívánt toouse.<br/><br/> hello forrásfelhőnek legalább egy VMM-gazdagépcsoportot kell tartalmaznia.<br/><br/> Minden VMM-felhőkben kell rendelkeznie a beállított hello Hyper-V kapacitás profil.<br/><br/> További VMM-felhőkben beállításával kapcsolatban lásd: [központi telepítése a virtuális gép magánfelhőben](https://technet.microsoft.com/en-us/system-center-docs/vmm/scenario/cloud-overview).
**Hyper-V** | Egy vagy több Hyper-V fürtök elsődleges és másodlagos VMM-felhőkben kell.<br/><br/> hello forrás Hyper-V fürt tartalmaznia kell legalább egy virtuális gépeket.<br/><br/> hello VMM-gazdagépcsoportot hello elsődleges és másodlagos helyek hello Hyper-V fürtök legalább egyikét kell tartalmaznia.<br/><br/>hello gazdagép és a cél Hyper-V kiszolgálók futnia kell a Windows Server 2012 vagy újabb hello Hyper-V szerepkör és hello legújabb frissítéseit telepítve.<br/><br/> Ha futtatja a Hyper-V fürt és a statikus IP cím alapú a fürt fürtszervező nem hozza létre automatikusan. Manuálisan kell konfigurálnia. További információkért lásd: [gazdagépfürtök Felkészülés a Hyper-V replika](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters).
**SAN-tároló** | Vendég fürtözött virtuális gépek iSCSI vagy a csatorna tárolóval, vagy a megosztott virtuális merevlemezeket (vhdx) használatával képes replikálni.<br/><br/> SAN-tömböket tartalmazó van szüksége: egy a hello elsődleges hely, a hello egy másodlagos helyen.<br/><br/> A hálózati infrastruktúra hello tömbök közötti kell beállítani. Társviszony-létesítés és replikációs úgy kell konfigurálni. Replikációs licencek hello tárolási tömb követelményeknek megfelelően kell beállítani.<br/><br/>Állítsa be, hogy a gazdagép iSCSI vagy Fibre Channel használatával kommunikálhat tárolók LUN számainak hello Hyper-V gazdakiszolgálók és hello tárolótömb közötti hálózati.<br/><br/> Ellenőrizze [támogatott tárolótömbök](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx).<br/><br/> Tárolási tömb gyártóktól származó SMI-S szolgáltatók kell telepíteni, és hello SAN-tömböket hello szolgáltató kell kezelnie. Hello szolgáltató függően toomanufacturer utasítások beállítása.<br/><br/>Győződjön meg arról, hogy hello tömb SMI-S szolgáltató a kiszolgálón, hogy hello VMM-kiszolgáló által elérhető IP-cím vagy FQDN hello hálózaton keresztül.<br/><br/> Minden egyes SAN-tömb rendelkeznie kell egy vagy több rendelkezésre álló tárolókészleteket.<br/><br/> VMM-kiszolgáló hello kell hello elsődleges tömb, hello másodlagos VMM-kiszolgálón kell valamint kezelheti és hello másodlagos tömb.
**Hálózatleképezés** | A hálózatleképezés, hogy a replikált virtuális gépek optimális kerülnek, a másodlagos Hyper-V gazdakiszolgálókra feladatátvétel után, és, hogy azokat a Virtuálisgép-hálózatok csatlakoztatott tooappropriate beállítása. Ha nem konfigurálja a hálózatra való leképezést, a replika virtuális gépek nem csatlakoztatott tooany hálózati feladatátvételt követően.<br/><br/> Győződjön meg arról, hogy a VMM-hálózatok helyesen vannak konfigurálva, hogy a hálózatra való leképezést a Site Recovery üzembe helyezése során állíthat be. A VMM-ben hello forrás Hyper-V gazdagépen lévő virtuális gépek hello csatlakoztatott tooa VMM Virtuálisgép-hálózathoz kell lennie. Erre a hálózatra kell csatolt tooa hello felhő társított logikai hálózat.<br/><br/> hello célfelhő rendelkeznie kell a megfelelő Virtuálisgép-hálózatot, és a csatolt tooa megfelelő logikai hálózatot hello célfelhő társított pedig kell.<br/><br/>.

## <a name="step-1-prepare-hello-vmm-infrastructure"></a>1. lépés: Hello a VMM infrastruktúra előkészítése
tooprepare a VMM infrastruktúra kell:

1. Ellenőrizze a VMM-felhőkben.
2. Integrálását, és a VMM a SAN-tároló besorolását.
3. Hozzon létre a logikai egységeket, és tárolót foglalni.
4. Replikációs csoportok létrehozása.
5. Állítsa be a Virtuálisgép-hálózatok.

### <a name="verify-vmm-clouds"></a>Ellenőrizze a VMM-felhő

Győződjön meg arról, hogy a VMM-felhőkben megfelelően vannak beállítva Site Recovery üzembe helyezése előtt.

### <a name="integrate-and-classify-san-storage-in-hello-vmm-fabric"></a>Integrálását, és a VMM háló rendszerű hello SAN-tároló besorolása

1. Hello VMM-konzolon nyissa meg túl**háló** > **tárolási** > **erőforrások hozzáadása** > **tárolóeszközök**.
2. A hello **tárolóeszközök hozzáadása** varázslóban válassza **válassza ki a tárolási szolgáltató típusát** válassza **SAN- és NAS-eszközök felderített és SMI-S szolgáltató által kezelt**.

    ![Szolgáltatótípus](./media/site-recovery-vmm-san/provider-type.png)

3. A hello **protokollt adja meg, és hello SMI-S társzolgáltató címét** lapon jelölje be **SMI-S CIMXML** , és adja meg a kapcsolódó toohello szolgáltató hello beállításai.
4. A **szolgáltatói IP-cím vagy FQDN** és **TCP/IP-port**, adja meg a kapcsolódó toohello szolgáltató hello beállításai. SMI-S CIMXML csak használhat egy SSL-kapcsolatot.

    ![Szolgáltató csatlakozás](./media/site-recovery-vmm-san/connect-settings.png)
5. A **futtató fiók**, adja meg a VMM futtató fiókja, amelyek hello szolgáltató eléréséhez, vagy hozzon létre egy fiókot.
6. A hello **információ összegyűjtése** lap, a VMM automatikusan kísérletet toodiscover-importálási hello tárolóeszköz információinak. tooretry felderítési kattintson **szolgáltató vizsgálata**. Hello felderítési folyamat sikeres, ha a hello felderített tárolótömbök, a tárolókészleteket, a gyártó, a modell és a kapacitás hello lapon találhatók.

    ![Tároló felderítése](./media/site-recovery-vmm-san/discover.png)
7. A **tárolási készletek tooplace felügyelet alatt válassza ki, és osztályozza**, válasszon hello tárolókészleteket, hogy a VMM kezeli, és rendeljen hozzájuk besorolást. Logikai egység az információkat importálja a hello tárolókészletek. Hozzon létre a LUN-alapú hello alkalmazásokban szükséges tooprotect, a kapacitás követelményeknek és a követelmények tooreplicate minek együtt.

    ![Tárolóbesorolás](./media/site-recovery-vmm-san/classify.png)

### <a name="create-luns-and-allocate-storage"></a>Hozzon létre a logikai egységeket, és a tároló foglalása

Hozzon létre a LUN-alapú hello alkalmazásokban szükséges tooprotect kapacitásigények és a követelmények tooreplicate minek együtt.

1. Hello tárolási hello VMM-háló jelenik meg, miután [LUN kiépítése](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-storage-host-groups#create-a-lun-in-vmm).

     > [!NOTE]
     > Virtuális merevlemezek nem adja hozzá a virtuális gép replikációs tooLUNs engedélyezett hello. Ha ezeket a logikai egységek nem a Site Recovery replikációs csoportban, akkor Site Recovery által nem észlelhető.
     >

2. Foglaljon le a tárolási kapacitás toohello Hyper-V gazdagépfürt, hogy a VMM virtuális gép kiépítése toohello adattárolás telepítheti:

   * Toohello tárolófürt lefoglalása, mielőtt tooallocate kell azt toohello mely hello fürtben található VMM-gazdagépcsoportot. További információkért lásd: [hogyan tooallocate tárolási logikai egység tooa gazdagépcsoport a VMM-ben](https://technet.microsoft.com/library/gg610686.aspx) és [hogyan a tooallocate tárolókészleteket tooa csoport a VMM-ben](https://technet.microsoft.com/library/gg610635.aspx).
   * Foglaljon le a tárolási kapacitás toohello fürt leírtak [hogyan tooconfigure tárolási egy Hyper-V gazdagépen a VMM-fürt](https://technet.microsoft.com/library/gg610692.aspx).

    ![Szolgáltatótípus](./media/site-recovery-vmm-san/provider-type.png)
3. A **protokollt adja meg, és hello SMI-S társzolgáltató címét**, jelölje be **SMI-S CIMXML**. Kapcsolódás toohello szolgáltató hello-beállítások megadása Az SSL-kapcsolat csak az SMI-S CIMXML is használhatja.

    ![Szolgáltató csatlakozás](./media/site-recovery-vmm-san/connect-settings.png)
4. A **futtató fiók**, adja meg a VMM futtató fiókja, amelyek hello szolgáltató eléréséhez, vagy hozzon létre egy fiókot.
5. A **információ összegyűjtése**, a VMM automatikusan kísérletet toodiscover-importálási hello tárolóeszköz információinak. Ha tooretry van szüksége, kattintson a **szolgáltató vizsgálata**. Ha hello felderítési folyamat sikeres, hello tárolótömbök tárolókészletek, a gyártó, a modell és a kapacitás hello oldalon felsorolt.

    ![Tároló felderítése](./media/site-recovery-vmm-san/discover.png)
7. A **tárolási készletek tooplace felügyelet alatt válassza ki, és osztályozza**, válasszon hello tárolókészleteket, hogy a VMM kezeli, és rendeljen hozzájuk besorolást. Logikai egység az információkat importálja a hello tárolókészletek.

    ![Tárolóbesorolás](./media/site-recovery-vmm-san/classify.png)


### <a name="create-replication-groups"></a>Replikációs csoportok létrehozása

Hozzon létre egy replikációs csoport, amely tartalmazza az összes hello LUN-okhoz együtt kell tooreplicate.

1. Hello VMM-konzolon nyissa meg a hello **replikációs csoportok** hello tárolási tömb tulajdonságai, és kattintson a lap **új**.
2. Hozzon létre hello replikációs csoportot.

    ![SAN-replikáláshoz](./media/site-recovery-vmm-san/rep-group.png)

### <a name="set-up-networks"></a>Hálózatok beállítása

Ha azt szeretné, hogy tooconfigure hálózatra való leképezés, hello a következő:

1. Lásd: a Site Recovery hálózatra való leképezés.
2. Virtuálisgép-hálózat előkészítése a VMM-ben:

   * [Állítson be logikai hálózatokat](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-network-logical-networks).
   * [Állítson be virtuálisgép-hálózatokat](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-network-vm-networks).


## <a name="step-2-create-a-vault"></a>2. lépés: A tároló létrehozása

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) hello VMM-kiszolgálóról szeretné tooregister hello tárolóban lévő állapottal.
2. Bontsa ki a **adatszolgáltatások** > **Recovery Services**, és kattintson a **Site Recovery-tároló**.
3. Kattintson az **Új létrehozása** > **Gyorslétrehozás** elemre.
4. A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban.
5. A **régió**, válassza ki a hello hello tároló földrajzi régióját. támogatott toocheck régiókban, lásd: [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Kattintson a **Tároló létrehozása** elemre.

    ![Új tároló](./media/site-recovery-vmm-san/create-vault.png)

Ellenőrizze, hogy hello állapot sáv tooconfirm hello tároló sikeresen létrejött. hello tároló tulajdonosaként **aktív** a fő hello **Recovery Services** lap.

### <a name="register-hello-vmm-servers"></a>Hello VMM-kiszolgálók regisztrálása

1. Nyissa meg hello **gyors üzembe helyezés** hello oldal **Recovery Services** lap. Gyors üzembe helyezési is megnyitható bármikor hello ikon kiválasztásával.

    ![Első lépések ikon](./media/site-recovery-vmm-san/quick-start-icon.png)
2. A hello legördülő mezőben válassza a **Hyper-V közötti helyszíni hely tömb replikációval**.

    ![Regisztrációs kulcs](./media/site-recovery-vmm-san/select-san.png)
3. A **előkészítése a VMM-kiszolgálók**, hello hello Azure Site Recovery Provider telepítőfájlja legújabb verziójának letöltéséhez.
4. Futtassa ezt a fájlt hello forrás VMM-kiszolgálón. Ha telepítése hello szolgáltató hello az első alkalommal van VMM fürtben üzemel, telepítse a szolgáltató hello aktív csomópontra, és Befejezés hello telepítési tooregister hello VMM-kiszolgáló hello tárolóban lévő állapottal. Majd telepítse hello szolgáltató hello más csomópontokra. Ha hello szolgáltató frissít, meg kell tooupgrade az összes olyan csomóponton, hogy azok rendelkeznek hello szolgáltató azonos verziója.
5. hello telepítője leellenőriz a követelményeket és a kérelmek engedély toostop hello a VMM szolgáltatás toobegin szolgáltató beállítása. hello szolgáltatás automatikusan újraindul a telepítő befejezése után. A VMM-fürthöz, a kért toostop hello fürtszerepkör lesz.
6. A **Microsoft Update**lapon kérheti a frissítések és szolgáltató frissítések tooyour Microsoft Update-szabályzatnak megfelelően lesznek telepítve.

    ![Microsoft Update](./media/site-recovery-vmm-san/ms-update.png)

7. Alapértelmezés szerint hello telepítés hello szolgáltató helye <SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin. Kattintson a **telepítése** toobegin.

    ![Telepítés helye](./media/site-recovery-vmm-san/install-location.png)

8. Hello szolgáltató telepítése után kattintson **regisztrálása** tooregister hello VMM-kiszolgáló hello tárolóban lévő állapottal.

    ![A telepítés befejeződött](./media/site-recovery-vmm-san/install-complete.png)

9. A **internetkapcsolat**, adja meg, hogy hello szolgáltató hogyan csatlakozzon a toohello Internet. Válassza ki **alapértelmezett rendszer proxybeállításainak használata** Ha azt szeretné, hogy toouse hello alapértelmezett internetkapcsolati beállításait hello kiszolgálón.

    ![Internetbeállítások](./media/site-recovery-vmm-san/proxy-details.png)

   * Ha egyéni proxyt toouse, beállítani hello szolgáltató telepítése előtt. Egyéni proxy beállításainak konfigurálásakor egy tesztet futtatja toocheck hello proxykapcsolatot.
   * Ha egyéni proxyt használ, vagy ha az alapértelmezett proxy hitelesítést igényel, meg kell adnia hello proxy adatait, beleértve a hello cím és port.
   * hello szükséges URL-címek hello VMM-kiszolgáló elérhetőnek kell lennie.
   * Ha egyéni proxyt használ, a VMM futtató fiókot (DRAProxyAccount) automatikusan létrejön a megadott hello proxy hitelesítő adatokat. Hello-proxy kiszolgáló konfigurálása, így a fiók elvégezhesse a hitelesítést. Módosíthatja a hello futtató fiók beállításait, a VMM-konzol hello (**beállítások** > **biztonsági** > **futtató fiókok**  >  **Draproxyaccount jelszavát**). Újra kell indítani a VMM szolgáltatás hello tootake hatás hello módosítása.
10. A **regisztrációs kulcs**, jelölje be hello kulcs letöltött hello portál és másolt toohello VMM-kiszolgálóról.
11. A **tároló neve**, ellenőrizze, melyik hello a kiszolgálót regisztrálni fogja hello tároló hello nevére.

    ![Kiszolgáló regisztrációja](./media/site-recovery-vmm-san/vault-creds.png)
12. hello titkosítási beállítást csak akkor használja a VMM-tooAzure replikációhoz. Figyelmen kívül hagyhatja azt.

    ![Kiszolgáló regisztrációja](./media/site-recovery-vmm-san/encrypt.png)
13. A **kiszolgálónév**, adjon meg egy rövid nevet tooidentify hello VMM-kiszolgáló hello tárolóban lévő állapottal. Fürtkonfiguráció adja meg a hello VMM-fürtszerepkör nevét.
14. A **kezdeti felhő metaadatainak szinkronizálása**, adja meg, hogy a kívánt toosynchronize metaadatok hello VMM-kiszolgálón futó összes felhő. Ez a művelet csak egyszer minden kiszolgálón toohappen van szüksége. Ha nem szeretné, toosynchronize összes felhő, hagyja bejelölve ezt a beállítást, és szinkronizálja egyenként a hello felhőtulajdonságok hello VMM-konzolon.

    ![Kiszolgáló regisztrációja](./media/site-recovery-vmm-san/friendly-name.png)

15. Kattintson a **következő** toocomplete hello folyamat. A regisztrációt követően az Azure Site Recovery által lekéri metaadatait hello VMM-kiszolgálóról. hello kiszolgálóhoz **kiszolgálók** > **VMM-kiszolgálókon** hello tárolóban lévő állapottal.

### <a name="command-line-installation"></a>Parancssori telepítés

hello Azure Site Recovery Provider a következő parancssori hello használatával is telepíthető. Ez a metódus lehet használt tooinstall hello szolgáltatót a Server Core for Windows Server 2012 R2.

1. Töltse le a hello szolgáltató telepítési és a regisztrációs kulcs tooa mappát. A mappa lehet például a következő: C:\ASR.
2. Hello a VMM szolgáltatás leállítása.
3. Hello Provider telepítőjének kibontásához. Futtassa az alábbi parancsokat rendszergazdaként:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Telepítse a szolgáltató hello:

        C:\ASR> setupdr.exe /i
5. Hello szolgáltató regisztrálása:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>         

Paraméterek:

* **/ Credentials**: hello helyet, mely hello a regisztrációs kulcs fájlját található a szükséges paraméter.  
* **/ FriendlyName**: kötelező paraméter a hello hello Hyper-V gazdakiszolgáló hello Azure Site Recovery portálon megjelenő nevét.
* **/ EncryptionEnabled**: nem kötelező paraméter csak akkor használja, ha a VMM tooAzure replikálásához.
* **/ proxyaddress**: nem kötelező paraméter, amely hello hello proxykiszolgáló címét.
* **/ proxyport**: nem kötelező paraméter, amely hello proxykiszolgáló hello port.
* **/ proxyusername**: nem kötelező paraméter, amely hello proxy felhasználónevét határozza meg, (ha hello proxy hitelesítést igényel).
* **/ proxypassword**: nem kötelező paraméter, amely hello jelszó hello proxykiszolgáló hitelesítéséhez (ha hello proxy hitelesítést igényel).

## <a name="step-3-map-storage-arrays-and-pools"></a>3. lépés: Térkép tárolótömbök és -készletek

Képezze le az elsődleges és másodlagos tömbök toospecify mely másodlagos tárolókészlet hello elsődleges készlet kap replikációs adatokat. Képezze le tárolási előtt konfigurálnia replikációs, mert hello leképezési adatok replikálási csoport védelmének engedélyezésekor.

Mielőtt elkezdené, ellenőrizze, hogy megjelennek-e a VMM-felhők hello tárolóban. Szolgáltató telepítése során az összes Felhő szinkronizálása során, vagy egy meghatározott felhő hello VMM-konzol szinkronizálása során a rendszer észleli a felhők.

1. Kattintson a **erőforrások** > **kiszolgálón történő tárolás** > **térkép forrás és cél tömbök**.
    ![Kiszolgáló regisztrálása](./media/site-recovery-vmm-san/storage-map.png)

2. Válassza ki a hello tárolótömbök hello elsődleges helyen, és hozzárendelheti toostorage tömbök hello másodlagos helyen. A **Tárolókészletek**, válasszon ki forrást, és a tárolási készlet toomap céloz.

    ![Kiszolgáló regisztrációja](./media/site-recovery-vmm-san/storage-map-pool.png)

## <a name="step-4-configure-replication-settings"></a>4. lépés: A replikáció beállításainak konfigurálása

Miután a VMM-kiszolgáló regisztrálva van, a felhő védelmi beállításainak konfigurálását.

1. A hello **gyors üzembe helyezés** kattintson **védelem beállítása a VMM-felhőkben**.
2. A hello **védett elemek** lapra, válassza ki a hello felhő **konfigurációs**.
3. A **cél**, jelölje be **VMM**.
4. A **célhelye**, jelölje be, amely kezeli a használni kívánt helyreállítási toouse hello felhő hello VMM-kiszolgáló.
5. A **céloz felhő**, válassza ki a kívánt toouse a virtuális gépek hello célfelhő.
   * Azt javasoljuk, hogy megfelel-e helyreállítási hello virtuális gépek védelme célfelhőt választotta.
   * Felhő csak tartozhat tooa egyetlen felhőpárnál – egy elsődleges vagy a cél felhő.
6. A Site Recovery ellenőrzi, hogy felhők hozzáférés tooSAN, és adott hello tárolására tömbök le vannak képezve.
7. Ha az ellenőrzés sikeres, a **replikációtípust**, jelölje be **SAN**.

Hello-beállítások mentése után egy feladat jön létre, amely képes figyelni az hello **feladatok** fülre. Beállítások módosíthatók hello **konfigurálása** fülre. Ha azt szeretné, toomodify hello célhelye vagy megcélzott felhőt, távolítsa el hello felhőkonfiguráció, majd konfigurálja újra a hello felhő.

## <a name="step-5-enable-network-mapping"></a>5. lépés: Hálózatleképezés engedélyezése

1. A hello **gyors üzembe helyezés** kattintson **hálózatok leképezése**.
2. Hello forrás VMM-kiszolgálót, majd válassza ki és hello cél VMM server toowhich hello hálózatok rendelendő számítógépekre. hello forrás hálózatok listáját és azok kapcsolódó célhálózatok jelennek meg. Üres értéket mutatja be, amelyek nincsenek leképezve hálózatok esetében. Kattintson a hello információk ikon következő toohello forrása és célja hálózati nevek tooview hello alhálózatok minden hálózathoz.
3. Válasszon egy hálózatot a **hálózati forrás**, és kattintson a **térkép**. hello szolgáltatás hello Virtuálisgép-hálózatokat a célkiszolgálón hello észleli, és megjeleníti őket.

    ![SAN-architektúra](./media/site-recovery-vmm-san/network-map1.png)
4. Válassza ki azt a Virtuálisgép-hálózatok hello hello cél VMM-kiszolgálóval.

    ![SAN-architektúra](./media/site-recovery-vmm-san/network-map2.png)

5. Amikor kiválaszt egy célhálózatot, hello hello Forráshálózat használó védett felhő jelennek meg. Az elérhető célkiszolgálók hálózatok is megjelennek. Azt javasoljuk, hogy bejelöli-e a replikáció használata esetén elérhető tooall hello felhők cél hálózaton.
6. Kattintson a hello pipa toocomplete hello megfeleltetési folyamat. Elindul egy feladat, amely nyomon követi az folyamatban van. Megtekintheti a hello **feladatok** fülre.

## <a name="step-6-enable-replication-for-replication-groups"></a>6. lépés: A következő replikációs csoportokhoz replikáció engedélyezése

Mielőtt engedélyezhetné a védelmet a virtuális gépek, szüksége tooenable replikációs replikációs csoportokat.

1. A hello **tulajdonságok** hello elsődleges felhő hello Site Recovery portálon, nyissa meg hello oldalán **virtuális gépek** fülre, és kattintson **replikációs csoport hozzáadása**.
2. Válassza ki egy vagy több VMM replikációs csoportokhoz társított hello felhő hello forrása és célja tömbök ellenőrizze, és hello replikációs frekvenciát adjon meg.

A Site Recovery, a VMM és a hello SMI-S szolgáltatók kiépítése hello cél hely tárolási logikai egységeket, és tárolási replikáció engedélyezése. Ha hello replikációs csoport már replikálódik, a Site Recovery a rendszer újból felhasználja hello meglévő replikációs kapcsolatot beállítani, és frissítések hello információkat.

## <a name="step-7-enable-protection-for-virtual-machines"></a>7. lépés: Virtuális gépek védelmének engedélyezése


Adattároló-csoport replikálása működik, ha engedélyezi a hello VMM-konzol virtuális gépek védelmét hello a következő módszerek egyikével:

* **Új virtuális gép**: a virtuális gépek létrehozásakor engedélyezze a replikálást, és társítsa hello VM hello replikálási csoporthoz.
  Ezzel a lehetőséggel VMM használja az intelligens elhelyezési toooptimally hely hello Virtuálisgép-tároló hello LUN-hello replikációs csoport. A Site Recovery koordinálja a hello árnyék hello másodlagos helyen lévő virtuális gép létrehozása, és lefoglalja a kapacitás, hogy a replika virtuális gépek a feladatátvételt követően is indítható.
* **Meglévő virtuális gép**: Ha egy virtuális gép már telepítve van a VMM-ben, engedélyezze a replikálást, és hajtsa végre egy áttelepítési tooa replikációs csoportjához. A VMM és a helyreállítás befejeztével észlelése után hello új virtuális Gépet, és kezelése, a Site Recovery szolgáltatásban – első lépések. Árnyék virtuális gép kerül létrehozásra hello másodlagos helyen, és úgy, hogy hello replika virtuális gép a feladatátvételt követően indítható lefoglalt kapacitás.

![Védelem engedélyezése](./media/site-recovery-vmm-san/enable-protect.png)

A replikáció engedélyezése virtuális gépek, után jelennek meg hello Site Recovery konzolján. Virtuális gép tulajdonságainak megtekintése, nyomon követheti állapotát, illetve követheti nyomon a több virtuális gépeket tartalmazó feladatátvételi replikációs csoportokat. A SAN-replikációra a replikációs csoporthoz társított összes virtuális gépet kell feladataikat együtt átadó. Ennek az az oka feladatátvételt hajt végre hello a tárolási rétegben először. A replikációs csoportnak megfelelően, és helyezze el egyszerre csak társított virtuális gépek fontos toogroup.

> [!NOTE]
> Miután a virtuális gépek replikációjának bekapcsolását, nem adja hozzá a VHD-k tooLUNs, kivéve, ha a Site Recovery replikációs csoportban találhatók. Virtuális merevlemezek csak észlelni fogja a Site Recovery által, ha a Site Recovery replikációs csoportban találhatók.
>
>

Előrehaladásának, beleértve a hello kezdeti replikálás, a hello **feladatok** fülre. Hello Védelemvéglegesítési feladatot futtatása után a hello virtuális gép készen áll a feladatátvételre.

![A virtuális gép védelme feladat](./media/site-recovery-vmm-san/job-props.png)

## <a name="step-8-test-hello-deployment"></a>8. lépés: Hello központi telepítés tesztelése

Tesztelje a központi telepítés toomake meg arról, hogy a virtuális gépek nagyobb a várt módon sikertelen lesz. toodo, a helyreállítási terv létrehozása, és feladatátvételi teszt futtatása.

1. A hello **helyreállítási tervek** lapra, majd **helyreállítási terv létrehozása**.
2. Adjon meg egy nevet hello helyreállítási tervet, és válassza ki a forrás és cél VMM-kiszolgálókon. hello forráskiszolgáló kell rendelkeznie, amelyek engedélyezve vannak a feladatátvétel és a helyreállítási virtuális gépek. Válassza ki **SAN** tooview csak felhők SAN-replikációhoz konfigurált.

    ![Helyreállítási terv létrehozása](./media/site-recovery-vmm-san/r-plan.png)

3. A **virtuális gépek kijelölése**, jelölje ki a replikációs csoportokat. Hello csoporthoz tartozó összes virtuális gép kerülnek toohello helyreállítási terv. A virtuális gépek hozzáadásakor toohello helyreállítási terv alapértelmezett csoport (1. csoport). Több olyan csoportot adhat hozzá, ha szükséges. A replikáció után virtuális gépek számozása toohello sorrendjének hello helyreállítási terv csoportok szerint.

    ![Válassza ki a virtuális gépek](./media/site-recovery-vmm-san/r-plan-vm.png)
4. Hello helyreállítási terv létrehozása után megjelenik a hello hello listában **helyreállítási tervek** fülre. Választhat hello tervet, és válassza a **feladatátvételi teszt**.
5. A hello **erősítse meg a feladatátvételi teszt** lapon jelölje be **nincs**. Engedélyezi ezt a beállítást, a replika virtuális gépek a feladatátvételt hello csatlakoztatott tooany hálózati nem lesz. Ellenőrzi, hogy hello virtuális gépek feladatok átadása a várt módon, de azt nem ellenőrzi a hello hálózati környezetben. Más hálózatkezelési lehetőségekkel kapcsolatban bővebben lásd: [Site Recovery feladatátvételi](site-recovery-failover.md).

    ![Válassza ki a teszthálózat](./media/site-recovery-vmm-san/test-fail1.png)

6. azonos gazdagép mely hello replika virtuális gép létezik hello gazdagépként hello hello teszteléshez használt virtuális gép jön létre. Mely hello replika virtuális Gépre toohello felhő nem adódik.
2. A replikáció után hello replika virtuális gép lesz mint hello elsődleges virtuális gép egy másik IP-címet. Ha címek bocsát ki a DHCP-Kiszolgálótól, akkor frissül automatikusan. Ha nem használja a DHCP és hello azonos címek, parancsfájlok néhány toorun van szüksége.
3. Futtassa a parancsfájlt tooretrieve hello IP-cím:

       $vm = Get-SCVirtualMachine -Name <VM_NAME>
       $na = $vm[0].VirtualNetworkAdapters>
       $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
       $ip.address  

4. Ez a minta parancsprogram tooupdate DNS futtatásához. Adja meg a lekért hello IP-címet.

       [string]$Zone,
       [string]$name,
       [string]$IP
       )
       $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
       $newrecord = $record.clone()
       $newrecord.RecordData[0].IPv4Address  =  $IP
       Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


## <a name="next-steps"></a>Következő lépések

A teszt feladatátvételi toocheck, hogy működik-e a környezet az elvárásoknak megfelelően futtatását, miután [Site Recovery feladatátvételi](site-recovery-failover.md) toolearn feladatátvételek különböző típusú.
