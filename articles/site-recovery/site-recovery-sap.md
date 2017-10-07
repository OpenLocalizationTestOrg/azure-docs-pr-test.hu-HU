---
title: "a többrétegű SAP NetWeaver alkalmazástelepítést Azure Site Recovery segítségével aaaProtect |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooprotect SAP NetWeaver alkalmazástelepítések Azure Site Recovery segítségével"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: manayar
ms.openlocfilehash: 34651c7b14d23a44005372f4f923c401e0224231
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protect-a-multi-tier-sap-netweaver-application-deployment-using-azure-site-recovery"></a>A többrétegű SAP NetWeaver alkalmazástelepítést Azure Site Recovery segítségével védelme

A legtöbb nagy- és közepes méretű SAP esetében van valamilyen vész-helyreállítási megoldás.  hatékony és testable vész-helyreállítási megoldások hello fontosságát több core üzleti folyamatok például SAP tooapplications áthelyezése növekedett.  Az Azure Site Recovery már tesztelt és részét képező SAP-alkalmazásokból, és meghaladja a legtöbb helyszíni vész-helyreállítási megoldást, egy alacsonyabb tulajdonosi költségek (TCO) mint versengő megoldások hello képességeit.
Az Azure Site Recovery segítségével:
* Engedélyezi a helyszíni-t futtató összetevők tooAzure replikálása SAP NetWeaver és NetWeaver termelési alkalmazások védelmét.
* Engedélyezi Azure,-t futtató replikálása Azure-adatközpontban összetevők tooanother SAP NetWeaver és NetWeaver termelési alkalmazások védelmét.
* Leegyszerűsíti a áttelepülés a felhőbe, a Site Recovery toomigrate a SAP telepítési tooAzure.
* Leegyszerűsíti az SAP projektek frissítését, tesztelését és prototípuskészítését, mivel a segítségével egy igény szerinti éles klón hozható létre az SAP alkalmazások teszteléséhez.

Ez a cikk ismerteti, hogyan tooprotect SAP NetWeaver alkalmazás üzembe helyezése [Azure Site Recovery](site-recovery-overview.md). Ez a cikk ismerteti hello ajánlott eljárásai védelmét a háromrétegű SAP NetWeaver központi telepítés Azure replikálása tooanother Azure datacenter használata az Azure Site Recovery szolgáltatásban hello támogatott forgatókönyvek és konfigurációk, és hogyan tooperform a feladatátvételeket, mindkettő tesztelése feladatátvételek (vészhelyreállítások részletezésének) vagy tényleges feladatátvételt.


## <a name="prerequisites"></a>Előfeltételek
Megkezdése előtt győződjön meg arról, hogy hello következő:

1. [A virtuális gép tooAzure replikálása](azure-to-azure-walkthrough-enable-replication.md)
2. Hogyan túl[egy helyreállítási hálózathoz. terv](site-recovery-azure-to-azure-networking-guidance.md)
3. [Ez a teszt feladatátvételi tooAzure](azure-to-azure-walkthrough-test-failover.md)
4. [Ez a feladatátvételi tooAzure](site-recovery-failover.md)
5. Hogyan túl[tartományvezérlő replikálása](site-recovery-active-directory.md)
6. Hogyan túl[SQL-kiszolgáló replikálása](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Támogatott helyzetek
Az Azure Site Recovery a vész-helyreállítási megoldást a következő forgatókönyvek hello valósíthatja meg:
* SAP rendszerekre a replikáló tooanother Azure datacenter (Azure-Azure DR), egy Azure-adatközpontban, a tervezett [Itt](https://aka.ms/asr-a2a-architecture).
* SAP rendszerek VMWare (vagy fizikai) kiszolgálókon futó helyszíni Azure datacenter (VMware-Azure DR), amely kell néhány további összetevők, mivel a tervezett replikációs tooa vész-Helyreállítási hely [Itt](https://aka.ms/asr-v2a-architecture).
* A Hyper-V SAP rendszerekre helyszíni Azure datacenter (DR Hyper-V-az-Azure-bA), amely kell néhány további összetevők, mivel a tervezett replikációs tooa vész-Helyreállítási hely [Itt](https://aka.ms/asr-h2a-architecture).

Ez a dokumentum hello első eset - Azure-Azure DR - toodemonstrate Azure Site Recovery SAP katasztrófa utáni helyreállítás szolgáltatásait használja. Mivel Azure Site Recovery replikációs alkalmazás független, hello leírt érték. a várt toohold az egyéb forgatókönyvek is.

### <a name="required-foundation-services"></a>Szükséges foundation szolgáltatások
A dokumentáció forgatókönyv minden lett foundation szolgáltatások telepítése a következő hello együtt telepíteni:
* Expressroute-on vagy a pont-pont virtuális magánhálózat (VPN)
* Azure-beli legalább egy Active Directory tartományvezérlő és DNS-kiszolgáló

Javasoljuk, hogy a fenti hello infrastruktúra meghatározott előzetes toodeploying Azure Site Recovery.


## <a name="typical-sap-application-deployment"></a>Tipikus SAP-alkalmazás telepítése
Nagy számú SAP-ügyfelek általában telepíteni 6 too20 egyedi SAP-alkalmazások között.  Ezeket az alkalmazásokat a legtöbb hello SAP NetWeaver ABAP vagy Java motorok alapulnak.  A core NetWeaver alkalmazásokat támogató a rendszer sok kisebb adott, nem NetWeaver SAP önálló motorok és általában valamilyen nem SAP-alkalmazásokból.  

Kritikus tooinventory hello SAP futó alkalmazásoknak a fekvő és toodetermine hello telepítési módban (rétegének 2 vagy 3 szintű), a verziók, a javítások, méretét, kavarog sebességét, és a lemez adatmegőrzési követelmények is.

![Telepítési minta](./media/site-recovery-sap/sap-typical-deployment.png)

hello SAP-adatbázisokhoz adatmegőrző réteget hello natív adatbázis-kezelő eszközök például az SQL Server AlwaysOn, Oracle DataGuard vagy HANA replikációs keresztül kell védeni. Azure Site Recovery által is nem védett hello ügyfél réteg, de ebben a rétegben, például a DNS-propagálás késleltetés, a biztonság és a távelérés toohello vész-Helyreállítási datacenter befolyásoló fontos tooconsider témaköröket.

Azure Site Recovery nem ajánlott megoldás hello alkalmazásréteg, többek között (A) SCS hello. Más alkalmazások például NetWeaver SAP-alkalmazásokból, és nem SAP-alkalmazásokból hello részét képezik a teljes SAP környezet és az Azure Site Recovery is védeni kell.

## <a name="replicate-virtual-machines"></a>Virtuális gépek replikálása
Hajtsa végre a [Ez az útmutató](azure-to-azure-walkthrough-enable-replication.md) replikálása az összes toostart hello SAP alkalmazás virtuális gépek toohello Azure DR-adatközpontban.

Ha egy statikus IP-cím használata esetén, amelyet a virtuális gép tootake hello hálózati illesztő kártyák szakaszának számítási és hálózati beállításainak hello hello IP is megadhat.

![Cél IP](./media/site-recovery-sap/sap-static-ip.png)


## <a name="creating-a-recovery-plan"></a>Helyreállítási terv létrehozása
A helyreállítási terv lehetővé teszi, hogy a különböző rétegek egy többrétegű alkalmazást, ezért az alkalmazás konzisztencia fenntartása hello feladatátvételi sorrendje. Hello ismertetett lépéseket követve [Itt](site-recovery-create-recovery-plans.md) többrétegű webalkalmazás esetén a helyreállítási terv létrehozása során.

### <a name="adding-scripts-toohello-recovery-plan"></a>A parancsfájlok toohello helyreállítási terv hozzáadása
Szükséges toodo hello Azure virtuális gépek post feladatátvételi és tesztelési célú feladatátvételi bizonyos műveleteket a alkalmazások toofunction megfelelően. Automatizálható a hello post feladatátvételi művelet, például a DNS-bejegyzés frissítéséhez, és adja hozzá a megfelelő parancsfájlokat hello helyreállítási tervben a kötések és kapcsolatok, módosítása [Ez a cikk](site-recovery-create-recovery-plans.md#add-scripts).

### <a name="dns-update"></a>DNS-frissítési
Ha hello DNS van konfigurálva a DNS dinamikus frissítést, majd a virtuális gépek általában frissítése hello DNS hello új IP-, ha elindítják a. Ha azt szeretné, tooadd egy explicit lépés tooupdate DNS hello az új IP-címek a virtuális gépek hello majd adja hozzá ezt a [tooupdate IP a DNS-parancsfájl](https://aka.ms/asr-dns-update) a post műveletek a helyreállítási terv csoportok.  

## <a name="example-azure-to-azure-deployment"></a>Azure-Azure központi telepítésre példát
Alább hello Azure Site Recovery Azure-Azure vész-Helyreállítási forgatókönyv hello diagramon ábrázolva:
* hello szingapúri (Azure Délkelet-Ázsia) az elsődleges adatközpont és vész-Helyreállítási datacenter hello Hongkong (Azure Kelet-Ázsia).  Ebben a forgatókönyvben helyi magas rendelkezésre állású azzal, hogy két virtuális gépeken futó SQL Server AlwaysOn szingapúri szinkron módban valósul meg.
* hello fájl megosztási ASC lehet használt tooprovide magas rendelkezésre ÁLLÁSÚ a hello SAP egyetlen ponton felmerülő hibákat. Fájl megosztási ASC nincs szükség a fürt megosztott lemez és alkalmazásokhoz, mint a SIOS nem kötelező megadni.
* Vész-Helyreállítási védelem hello DBMS réteg aszinkron replikáció használatával érhető el.
* Ebből a forgatókönyvből megtudhatja "szimmetrikus DR" – használt kifejezés egy vész-Helyreállítási megoldás, amely az üzemi pontos replikájának toodescribe, ezért hello vész-Helyreállítási SQL Server-megoldását rendelkezik-e helyi magas rendelkezésre állású. hello szimmetrikus vész-Helyreállítási használata nem kötelező hello adatbázis réteg, és számos ügyfél kihasználhatják felhő központi telepítések toobuild egy helyi magas rendelkezésre állási csomópontot hello rugalmasan gyorsan vész-Helyreállítási esemény után.
* hello ábra szemlélteti a hello SAP NetWeaver ASC és az Azure Site Recovery által replikált kiszolgáló alkalmazásréteg.

![Replikációs forgatókönyv](./media/site-recovery-sap/sap-replication-scenario.png)

## <a name="doing-a-test-failover"></a>A teszt feladatátvétel
Hajtsa végre a [Ez az útmutató](azure-to-azure-walkthrough-test-failover.md) toodo feladatátvételi tesztet.

1.  Nyissa meg tooAzure portálon, és válassza a Recovery Services-tároló.
2.  Kattintson a hello helyreállítási tervben készült SAP-alkalmazásokból (s).
3.  Kattintson a "Test Failover".
4.  Válassza ki a helyreállítási pont és az Azure-beli virtuális hálózat toostart hello teszt feladatátvételi folyamat.
5.  Hello másodlagos környezetben működik, ha az érvényesítést végezheti el.
6.  Ha hello ellenőrzés befejeződött, kattintson a "Karbantartása a feladatátvételi teszt" és a tooclean hello feladatátvételi környezet.

## <a name="doing-a-failover"></a>A feladatátvétel
Hajtsa végre a [Ez az útmutató](site-recovery-failover.md) Ha feladatátvételt végez.

1.  Nyissa meg tooAzure portálon, és válassza a Recovery Services-tároló.
2.  Kattintson a létrehozott SAP alkalmazás(ok) hello helyreállítási terv.
3.  Kattintson a "Failover".
4.  Válassza ki a helyreállítási pont toostart hello feladatátvételi folyamatot.

## <a name="next-steps"></a>Következő lépések
A vész-helyreállítási megoldást SAP NetWeaver üzembe helyezése az Azure Site Recovery létrehozásával kapcsolatos további [e tanulmány](http://aka.ms/asr-sap). hello tanulmány is ismerteti, amelyek különböző SAP-architektúra javaslatok, támogatott alkalmazások és a Virtuálisgép-típusokon az SAP, az Azure-on, és ismerteti lehetséges tesztelési terveket, a vész-helyreállítási megoldás.

További információ [replikál a többi munkaterhelését](site-recovery-workload.md) Site Recovery segítségével.
