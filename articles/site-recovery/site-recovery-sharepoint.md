---
title: "egy Azure Site Recovery segítségével többrétegű SharePoint alkalmazás aaaReplicate |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooreplicate egy többrétegű SharePoint-alkalmazást, az Azure Site Recovery képességeit."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: sutalasi
ms.openlocfilehash: d856034ac2a3c95b0c1f0cf85e62c4e7a5a3210f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-sharepoint-application-for-disaster-recovery-using-azure-site-recovery"></a>Azure Site Recovery segítségével vész-helyreállítási egy többrétegű SharePoint alkalmazás replikálása

Ez a cikk részletesen ismerteti, hogyan egy SharePoint használó tooprotect [Azure Site Recovery](site-recovery-overview.md).


## <a name="overview"></a>Áttekintés

A Microsoft SharePoint egy hatékony alkalmazás, amelynek segítségével egy csoport vagy részleg rendezéséhez, együttműködés és az információkat. SharePoint intranetes portálok, dokumentum és fájlkezelés, együttműködés, közösségi hálózatokkal, extranetek, webhelyek, vállalati keresési és üzleti intelligencia biztosít. Azt is, rendszerintegráció integrálása és munkafolyamat automatizálási lehetőségek körét. Általában a szervezetek fontolja meg az 1. rétegbeli alkalmazás-és nagybetűket toodowntime és az adatok elvesztése.

A Microsoft SharePoint jelenleg nem biztosít bármely out-of-az-box vész-helyreállítási funkciók. Hello típust és egy olyan vészhelyzet esetén a méretezési, függetlenül a helyreállítási helyezési hello hello farm helyreállítható készenléti adatközpontban. Készenléti adatközpontok forgatókönyvek, ahol helyi redundáns rendszerek és a biztonsági mentések nem tudja elhárítani az hello kimaradás hello elsődleges data Center szükségesek.

Egy jó vész-helyreállítási megoldást körül hello összetett alkalmazási architektúrákban például SharePoint helyreállítási tervek modellezési engedélyezni kell. Ez lehet hello teszi testre szabott tooadd lépéseket toohandle alkalmazástársítások között különböző szinteket, és így biztosítható a egy kattintással indítható feladatátvételt egy alacsonyabb RTO hello esemény egy olyan vészhelyzet esetén a.

Ez a cikk részletesen ismerteti, hogyan egy SharePoint használó tooprotect [Azure Site Recovery](site-recovery-overview.md). Ez a cikk a három réteg SharePoint alkalmazás tooAzure, hogyan teheti a vész-helyreállítási részletezéshez és feladatátvételi hello alkalmazás tooAzure adhat replikálása ajánlott eljárásai szakaszában.

Figyelheti az alábbi videó helyreállítani egy több réteg alkalmazás tooAzure hello.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/Disaster-Recovery-of-load-balanced-multi-tier-applications-using-Azure-Site-Recovery/player]


## <a name="prerequisites"></a>Előfeltételek

Megkezdése előtt győződjön meg arról, hogy hello következő:

1. [A virtuális gép tooAzure replikálása](site-recovery-vmware-to-azure.md)
2. Hogyan túl[egy helyreállítási hálózathoz. terv](site-recovery-network-design.md)
3. [Ez a teszt feladatátvételi tooAzure](site-recovery-test-failover-to-azure.md)
4. [Ez a feladatátvételi tooAzure](site-recovery-failover.md)
5. Hogyan túl[tartományvezérlő replikálása](site-recovery-active-directory.md)
6. Hogyan túl[SQL-kiszolgáló replikálása](site-recovery-sql.md)

## <a name="sharepoint-architecture"></a>SharePoint-architektúra

SharePoint rétegzett topológiákat és a kiszolgálói szerepkörök tooimplement farm kialakítást, amely megfelel a konkrét célokat és célok segítségével egy vagy több kiszolgálón is telepíthető. Egy tipikus nagy, nagy igény SharePoint-kiszolgálófarm nagyszámú egyidejű felhasználót és nagyszámú tartalomelem támogató szolgáltatás csoportosítás használata a méretezhetőség stratégia részeként. Ez a megközelítés szolgáltatásokat futtató található dedikált kiszolgálókra, ezek a szolgáltatások csoportosítása, és majd kiterjesztése hello kiszolgálókra csoportként. hello következő topológia szemlélteti hello szolgáltatást és a kiszolgáló SharePoint-kiszolgálófarm három rétegekhez csoportosítása. Tekintse meg az tooSharePoint dokumentáció és a termék sor architektúrák különböző SharePoint topológiák részletes útmutatást. A SharePoint 2013 fejlesztésével kapcsolatos további részletei [Ez a dokumentum](https://technet.microsoft.com/en-us/library/cc303422.aspx).



![Telepítési minta 1](./media/site-recovery-sharepoint/sharepointarch.png)


## <a name="site-recovery-support"></a>Webhely-helyreállítási támogatás

Ez a cikk létrehozásához, a VMware virtuális gépek, a Windows Server 2012 R2 Enterprise használták. A SharePoint 2013 Enterprise edition és az SQL server 2014 Enterprise edition használták. Mivel a Site Recovery replikációs alkalmazás független, hello javaslatok itt megadott várható toohold a, valamint a következő forgatókönyvek esetén.

### <a name="source-and-target"></a>Forrása és célja

**A forgatókönyv** | **másodlagos hely tooa** | **tooAzure**
--- | --- | ---
**Hyper-V** | Igen | Igen
**VMware** | Igen | Igen
**Fizikai kiszolgáló** | Igen | Igen

### <a name="sharepoint-versions"></a>SharePoint-verziók
a következő SharePoint server-verziók hello támogatott.

* A SharePoint server 2013 Standard
* SharePoint server 2013 Enterprise
* A SharePoint server 2016 Standard
* SharePoint server 2016 Enterprise

### <a name="things-tookeep-in-mind"></a>Dolgot tookeep szem előtt

Ha használ egy megosztott lemez alapú fürt bármely réteg az alkalmazásban, akkor nem lesz képes toouse Site Recovery replikációs tooreplicate kell ezeket a virtuális gépeket. Hello alkalmazás által biztosított natív replikációt használ, és ezután egy [helyreállítási terv](site-recovery-create-recovery-plans.md) toofailover minden csomagban.

## <a name="replicating-virtual-machines"></a>Virtuális gépek replikálása

Hajtsa végre a [Ez az útmutató](site-recovery-vmware-to-azure.md) toostart hello virtuális gép tooAzure replikálása.

* Ha hello replikáció befejeződött, győződjön meg arról, nyissa meg az egyes rétegek tooeach virtuális gép, és válassza ki az azonos rendelkezésre állási csoport "replikált elemek > Beállítások > Tulajdonságok > Számítás és hálózat". Például ha a webes réteg 3 virtuális gép van, győződjön meg arról 3 virtuális gépeken minden hello konfigurálva toobe részét egyazon rendelkezésre állási csoportban az Azure-ban.

    ![Set-rendelkezésre állási-csoport](./media/site-recovery-sharepoint/select-av-set.png)

* Active Directory és a DNS védelméről, című témakörben olvashat útmutatót túl[Active Directory védelmére és a DNS-](site-recovery-active-directory.md) dokumentum.

* Védett működő SQL server adatbázis-rétegből, című témakörben olvashat útmutatót túl[SQL Server védelme](site-recovery-active-directory.md) dokumentum.

## <a name="networking-configuration"></a>Hálózati konfigurációja

### <a name="network-properties"></a>Hálózat tulajdonságai

* Hello alkalmazás és a webes réteg virtuális gépek hálózati beállítások konfigurálása a Azure-portálon, hogy hello virtuális gépek csatolt toohello jobb vész-Helyreállítási hálózati beolvasása a feladatátvételt követően.

    ![Válassza ki a hálózati](./media/site-recovery-sharepoint/select-network.png)


* Ha statikus IP-címet használ, majd adja meg, amelyet a virtuális gép tootake a hello hello hello IP **cél IP-címet** mező

    ![Statikus IP-cím beállítása](./media/site-recovery-sharepoint/set-static-ip.png)

### <a name="dns-and-traffic-routing"></a>DNS- és a forgalom-Útválasztás

Az internetre irányuló helyek ["Priority" típusú Traffic Manager-profil létrehozása](../traffic-manager/traffic-manager-create-profile.md) a hello Azure-előfizetés. Majd konfigurálja a DNS és a Traffic Manager-profilt a következő módon hello.


| **Ha** | **Forrás** | **Cél**|
| --- | --- | --- |
| Nyilvános DNS-ben | Nyilvános DNS-ben, a SharePoint-webhelyekhez <br/><br/> Például: sharepoint.contoso.com | Traffic Manager <br/><br/> contososharepoint.trafficmanager.NET |
| A helyi DNS | sharepointonprem.contoso.com | Nyilvános IP-cím hello helyszíni farm |


A Traffic Manager-profil hello [elsődleges és a helyreállítási végpontokat hoz létre a hello](../traffic-manager/traffic-manager-configure-priority-routing-method.md). Külső végpont hello használata a helyszíni végpont és az Azure-végpont nyilvános IP-cím. Győződjön meg arról, hogy hello prioritás a magasabb tooon helyszíni végpont van beállítva.

Állomás egy adott portot (például 800) hello SharePoint webes réteg ahhoz, hogy a Traffic Manager tooautomatically tesztoldalt észleli a rendelkezésre állási post feladatátvétele. Ez a megoldás, abban az esetben nem engedélyezhető a névtelen hitelesítés bármely, a SharePoint-webhelyek.

[Hello Traffic Manager-profil konfigurálása](../traffic-manager/traffic-manager-configure-priority-routing-method.md) az alábbi beállítások hello.

* Útválasztási módszer - "Priority"
* DNS-toolive (TTL) - "30 másodperc' idő
* Végpont-figyelő beállításai – Ha engedélyezi a névtelen hitelesítés adhat egy adott webhelyre végpont. Vagy egy adott portot (például 800) tesztoldalt is használhatja.

## <a name="creating-a-recovery-plan"></a>Helyreállítási terv létrehozása

A helyreállítási terv lehetővé teszi, hogy a különböző rétegek egy többrétegű alkalmazást, ezért az alkalmazás konzisztencia fenntartása hello feladatátvételi sorrendje. Hajtsa végre a következő lépések hello többrétegű webalkalmazás esetén a helyreállítási terv létrehozása során. [További információ a helyreállítási terv létrehozása](site-recovery-runbook-automation.md#customize-the-recovery-plan).

### <a name="adding-virtual-machines-toofailover-groups"></a>Virtuális gépek toofailover csoportok hozzáadása

1. Alkalmazás hozzáadása hello és webes réteg virtuális gépek helyreállítási terv létrehozása.
2. Kattintson a "Testreszabás" toogroup hello virtuális gépeket. Alapértelmezés szerint az összes virtuális gép "Csoport 1" részét képezik.

    ![Függő Entitás testreszabása](./media/site-recovery-sharepoint/rp-groups.png)

3. Hozzon létre egy másik csoportot (csoport % 2), és helyezze át a hello webes réteg virtuális gépek hello új csoportba. Az alkalmazás réteg virtuális gépek csoport "% 1" részének kell lennie, és webes réteg virtuális gépek csoport "% 2" részének kell lennie. Ez az, hogy a réteg virtuális gépek indítsa el a webes réteg virtuális gépeket először követ App hello tooensure.


### <a name="adding-scripts-toohello-recovery-plan"></a>A parancsfájlok toohello helyreállítási terv hozzáadása

Az Automation-fiók hello "Telepítése tooAzure" gombra kattint a leggyakrabban használt hello Azure Site Recovery parancsfájlok is telepíthet. Ha bármely közzétett parancsfájl használ, győződjön meg arról, hajtsa végre a parancsfájl hello hello útmutatást.

[![TooAzure telepítése](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. Vegyen fel egy előtti parancsfájlművelet too'Group 1' toofailover SQL rendelkezésre állási csoportot. Parancsfájllal hello "ASR-SQL-FailoverAG" hello minta parancsfájlokat tesznek közzé. Győződjön meg arról, kövesse az útmutatást hello hello parancsfájl, és módosítsa szükséges hello hello parancsfájl megfelelően.

    ![Adja hozzá-AG-parancsfájl--1. lépés](./media/site-recovery-sharepoint/add-ag-script-step1.png)

    ![Adja hozzá-AG-parancsfájl-– 2. lépés](./media/site-recovery-sharepoint/add-ag-script-step2.png)

2. Adja hozzá a feladás egy vagy több művelet parancsfájl tooattach hello a terheléselosztó webes réteg (csoport 2) a virtuális gépek a feladatátvételt. Parancsfájllal hello "ASR-AddSingleLoadBalancer" hello minta parancsfájlokat tesznek közzé. Győződjön meg arról, kövesse az útmutatást hello hello parancsfájl, és módosítsa szükséges hello hello parancsfájl megfelelően.

    ![Adja hozzá-LB-parancsfájl--1. lépés](./media/site-recovery-sharepoint/add-lb-script-step1.png)

    ![Adja hozzá-LB-parancsfájl-– 2. lépés](./media/site-recovery-sharepoint/add-lb-script-step2.png)

3. A manuális lépés tooupdate hello DNS rekordok toopoint toohello új farmhoz hozzáadása az Azure-ban.

    * Az internetes webhelyek nincs DNS-frissítések szükséges post feladatátvételi. Hello "Hálózat útmutatást" szakasz tooconfigure Traffic Manager hello lépéseit kövesse. Hello Traffic Manager-profil beállítása hello előző szakaszban leírtak szerint, ha a hello Azure virtuális gép adjon hozzá egy parancsfájl tooopen dummy portot (800 hello példában).

    * Belső webhelyet vegyen fel egy manuális lépés tooupdate hello DNS rekord toopoint toohello új webes réteg virtuális gép load balancer IP.

4. Új keresési szolgáltatás indítására vagy egy manuális lépés toorestore keresőalkalmazás hozzáadni egy biztonsági másolatból.

5. Keresési szolgáltatásalkalmazás visszaállítása egy biztonsági másolatból, hajtsa végre az alábbi lépéseket.

    * Ez a módszer feltételezi, hogy a keresési szolgáltatásalkalmazás hello egy biztonsági másolat hello katasztrofális esemény előtt és biztonsági mentését végző hello elérhető hello vész-Helyreállítási helyen.
    * Ez könnyen elérhető hello biztonsági mentés ütemezése (például naponta egyszer), majd az eljárás tooplace hello másolat hello vész-Helyreállítási helyen. Másolás eljárások tartalmazhatnak parancsfájlalapú programok például AzCopy (Azure másolás) vagy az elosztott fájlrendszer replikációs szolgáltatása (elosztott szolgáltatások fájlreplikáció) beállítása.
    * Most, hogy hello SharePoint-farm fut, nyissa meg a központi felügyelet "Biztonsági mentése és visszaállítása" hello, és válassza ki a visszaállítást. hello visszaállítási interrogates hello megadott biztonsági mentési helyre (esetleg tooupdate hello érték). Válassza ki azt szeretné, hogy toorestore a hello szolgáltatás keresőalkalmazás biztonsági másolatot.
    * Keresési helyreáll. Ne feledje, hogy a visszaállítási hello vár toofind hello azonos topológia (azonos számú kiszolgálók) és azonos rögzített meghajtóbetűjelei toothose kiszolgálók. További információkért lásd: ["Visszaállítása keresési szolgáltatásalkalmazás a SharePoint 2013"](https://technet.microsoft.com/library/ee748654.aspx) dokumentum.


6. Egy új szolgáltatás keresőalkalmazás kezdve, hajtsa végre az alábbi lépéseket.

    * Ez a módszer feltételezi, hogy a hello "Keresési Adminisztráció" adatbázis biztonsági másolatának elérhető hello vész-Helyreállítási helyen.
    * Hello más keresési szolgáltatásalkalmazás adatbázisok nem replikálja, mivel szükségük toobe újból létrehozza. toodo Igen, keresse meg a felügyeleti tooCentral és hello keresési szolgáltatásalkalmazás törlése. Egyetlen kiszolgálón, mely gazdagépen hello Search-Index hello index fájlok törlése.
    * Hozza létre újból hello keresési szolgáltatásalkalmazás, és ez újra létrehozza a hello adatbázisok. Toohave javasoljuk egy előkészített parancsfájlt, amely újra létrehozza a szolgáltatásalkalmazás mivel ez nem lehetséges tooperform minden művelet hello grafikus felhasználói felületen keresztül. Például hello index meghajtóhelyét és hello keresési topológia beállításával csak akkor lehetséges SharePoint PowerShell-parancsmagok használatával. Hello Windows PowerShell-parancsmag visszaállítási-SPEnterpriseSearchServiceApplication, és adja meg a hello naplóküldés és keresési felügyeleti adatbázis, Search_Service__DB replikálni. Ez a parancsmag hello keresési konfigurációt, a séma, a felügyelt tulajdonságok, a szabályok és a források biztosít, és létrehozza a hello alapértelmezett készletét más összetevőket.
    * Keresési szolgáltatás alkalmazás hello kell hozza létre újra, miután minden tartalomforrás toorestore hello – keresés szolgáltatást a teljes bejárás kell elindítani. Hello helyszíni kiszolgálófarmon, például a keresési javaslatok tárolt analytics adatok elvesznek.

7. Amennyiben az összes hello lépéseket, mentés hello helyreállítási tervet, és hello végső helyreállítási terv a következőképpen néznek.

    ![Mentett RP](./media/site-recovery-sharepoint/saved-rp.png)

## <a name="doing-a-test-failover"></a>A teszt feladatátvétel
Hajtsa végre a [Ez az útmutató](site-recovery-test-failover-to-azure.md) toodo feladatátvételi tesztet.

1.  Nyissa meg tooAzure portálon, és válassza a helyreállítási szolgáltatás tárolójából.
2.  Kattintson a SharePoint-alkalmazáshoz létrehozott hello helyreállítási terv.
3.  Kattintson a "Test Failover".
4.  Válassza ki a helyreállítási pont és az Azure-beli virtuális hálózat toostart hello teszt feladatátvételi folyamat.
5.  Hello másodlagos környezetben működik, ha az érvényesítést végezheti el.
6.  Ha hello ellenőrzés befejeződött, "Tisztítás feladatátvételi teszt" a helyreállítási terv hello kattinthat, és hello teszt feladatátvételi környezet karbantartása.

A teszt feladatátvétel ad útmutatást és a DNS, tekintse meg a túl[feladatátvételi szempontokat részletező cikket az Active Directory és a DNS-](site-recovery-active-directory.md#test-failover-considerations) dokumentum.

Feladatátvétel tesztelése az SQL mindig rendelkezésre állási CSOPORTJAIRÓL, című témakörben olvashat útmutatót túl[feladatátvételi teszt során az SQL Server Always On](site-recovery-sql.md#steps-to-do-a-test-failover) dokumentum.

## <a name="doing-a-failover"></a>A feladatátvétel
Hajtsa végre a [Ez az útmutató](site-recovery-failover.md) a a feladatátvétel.

1.  Nyissa meg tooAzure portálon, és válassza a Recovery Services-tároló.
2.  Kattintson a SharePoint-alkalmazáshoz létrehozott hello helyreállítási terv.
3.  Kattintson a "Failover".
4.  Válassza ki a helyreállítási pont toostart hello feladatátvételi folyamatot.

## <a name="next-steps"></a>Következő lépések
További tudnivalók [replikál a többi alkalmazás](site-recovery-workload.md) Site Recovery segítségével.
