---
title: "Ön aaaWhat munkaterhelések is az Azure Site Recovery védelmét?"
description: "Az Azure Site Recovery védi a munkaterheléseket és alkalmazásokat hello replikációjának, feladatátvételének és a helyszíni virtuális gépek és fizikai kiszolgálók tooAzure vagy tooa másodlagos helyszíni helyre történő összehangolásával"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: 4953948f-26c0-4699-8fe7-59d3bfc1d3da
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/08/2017
ms.author: raynew
ms.openlocfilehash: cab2e1ce3c2b7b2c5f899d957219f5c12eb5965c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>Milyen számítási feladatokat tud védeni az Azure Site Recovery?
Ez a cikk ismerteti a munkaterheléseket és alkalmazásokat az Azure Site Recovery szolgáltatás hello replikálhatja.

Ez a cikk vagy a hello hello alsó megjegyzések vagy kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Áttekintés
A szervezetek kell üzletmenet-folytonosságának és vészhelyreállítási (BCDR) helyreállítási stratégia tookeep munkaterhelések és adatok biztonságos és használható tervezett és nem tervezett leállások során, és tooregular működésre minél hamarabb helyreállítani.

Az Azure-szolgáltatások tooyour BCDR stratégia járul. A Site Recovery segítségével, akkor telepítheti alkalmazástudatos replikációt toohello felhő, vagy tooa másodlagos helyen. E az alkalmazások olyan Windows vagy Linux-alapú, futó, fizikai kiszolgálókon, VMware vagy a Hyper-V, a Site Recovery tooorchestrate replikáció, vész-helyreállítási tesztelheti, és átvételek és feladat-visszavétel futtassa.

A Site Recovery számos Microsoft-alkalmazással (például SharePoint, Exchange, Dynamics, SQL Server és Active Directory) képes együttműködni. A Microsoft továbbá szorosan együttműködik az olyan vezető szállítókkal, mint az Oracle, a SAP, az IBM vagy a Red Hat. A replikációs megoldásokat alkalmazásonként szabhatja testre.

## <a name="why-use-site-recovery-for-application-replication"></a>Miért előnyös a Site Recovery használata az alkalmazásreplikációhoz?
Site Recovery segíti Önt tooapplication szintű védelmi és helyreállítási az alábbiak szerint:

* Alkalmazásfüggetlen, így egy támogatott gépen futó bármilyen számítási feladatok replikációját biztosítja.
* Közel szinkron replikáció, mint 30 másodperces toomeet hello igényeinek legfontosabb üzleti alkalmazásokhoz időkorlátjával.
* Alkalmazáskonzisztens pillanatképeket rögzít egy- vagy többszintű alkalmazásokról.
* Együttműködik az SQL Server AlwaysOn szolgáltatással, és számos más alkalmazásszintű replikációs technológiát is képes felhasználni (például AD-replikáció, SQL AlwaysOn, Exchange adatbázis-elérhetőségi csoportok (DAG) és Oracle Data Guard).
* Rugalmas helyreállítási tervek, egyetlen kattintással egész alkalmazáscsoportokat toorecover engedélyezése és tooinclude külső parancsprogramok és manuális műveletek közé tartozik a hello tervben.
* Fejlett hálózatkezelési és az Azure Site Recovery toosimplify app alkalmazáshálózati követelményeket, ideértve a hello képességét tooreserve IP-címek konfigurálása terheléselosztás és az integráció az Azure Traffic Manager alacsony RTO hálózatváltást garantál.
* A szolgáltatás gazdag, éles használatra kész és alkalmazásspecifikus parancsprogramokat tartalmazó automatizációs kódtárat tartalmaz, amely letölthető, és beépíthető a helyreállítási tervekbe.

## <a name="workload-summary"></a>A számítási feladatok összefoglalása
A Site Recovery a támogatott gépeken futó bármilyen alkalmazást képes replikálni. Ezen kívül is végrehajtottunk termék csapatok toocarry ki további alkalmazásspecifikus teszteket.

| **Számítási feladat** | **Másodlagos hely replikálást a Hyper-V virtuális gépek tooa** | **Hyper-V virtuális gépek tooAzure replikálása** | **VMware virtuális gépek tooa másodlagos helyre replikálni.** | **VMware virtuális gépek tooAzure replikálása** |
| --- | --- | --- | --- | --- |
| Active Directory, DNS |I |I |I |I |
| Webalkalmazások (IIS, SQL) |I |I |I |I |
| System Center Operations Manager |I |I |I |I |
| SharePoint |I |I |I |I |
| SAP<br/><br/>SAP-hely tooAzure nem fürt replikálása |I (a Microsoft által végzett tesztek alapján) |I (a Microsoft által végzett tesztek alapján) |I (a Microsoft által végzett tesztek alapján) |I (a Microsoft által végzett tesztek alapján) |
| Exchange (nem DAG) |I |I |I |I |
| Távoli asztal/VDI |I |I |I |N/A |
| Linux (operációs rendszer és alkalmazások) |I (a Microsoft által végzett tesztek alapján) |I (a Microsoft által végzett tesztek alapján) |I (a Microsoft által végzett tesztek alapján) |I (a Microsoft által végzett tesztek alapján) |
| Dynamics AX |I |I |I |I |
| Dynamics CRM |I |Hamarosan |I |Hamarosan elérhető |
| Oracle |I (a Microsoft által végzett tesztek alapján) |I (a Microsoft által végzett tesztek alapján) |I (a Microsoft által végzett tesztek alapján) |I (a Microsoft által végzett tesztek alapján) |
| Windows fájlkiszolgáló |I |I |I |I |
| Citrix XenApp és XenDesktop |N/A |I |N/A |I |

## <a name="replicate-active-directory-and-dns"></a>Active Directory és DNS replikálása
Az Active Directory és a DNS-infrastruktúra olyan alapvető toomost vállalati alkalmazások. A vészhelyreállítás során lesz tooprotect kell, és infrastruktúra e komponenseinek helyreállítani a számítási feladatok és alkalmazások helyreállítása előtt.

Az Active Directory és a DNS-Site Recovery toocreate egy teljes automatizált vészhelyreállítási tervet is használhatja. Például ha toofail SharePoint és a másodlagos hely elsődleges tooa SAP, beállíthatja a helyreállítási terv, amely átadja a feladatokat az Active Directory először, és majd egy kiegészítő alkalmazásspecifikus helyreállítási terv toofail hello keresztül aktív a használó alkalmazások Könyvtár.

[Itt részletesebben tájékozódhat](site-recovery-active-directory.md) az Active Directory és a DNS védelméről.

## <a name="protect-sql-server"></a>Az SQL Server védelme
Az SQL Server adatszolgáltatási alapot nyújt az üzleti alkalmazások helyi adatközpontban működő adatszolgáltatásai számára.  Helyreállítás az SQL Server elérhető HA/DR technológiák, SQL Server használó többrétegű vállalati alkalmazások tooprotect együtt is használható. A Site Recovery a következőket biztosítja:

* Egyszerű és költséghatékony vészhelyreállítási megoldás az SQL Serverhez. Több verziójának és kiadásának SQL Server önálló kiszolgálók és fürtök, tooAzure vagy tooa másodlagos helyre replikálni.  
* Az SQL AlwaysOn rendelkezésre állási csoportok, a toomanage feladatátvételi és a feladat-visszavétel az Azure Site Recovery helyreállítási terveiben integrációját.
* Végpontok közötti helyreállítási tervek hello a kérelmet, beleértve az SQL Server-adatbázisok hello összes rétege.
* Magas terhelésekhez méretezhető SQL Server Site Recovery-vel az Azure nagyobb méretű IaaS virtuális gépekkel történő teljesítménynöveléssel.
* Az SQL Server vészhelyreállítási funkcióinak egyszerű tesztelése. Futtassa a feladatátvételi tesztet tooanalyze adatokat, és futtathat megfelelőség-ellenőrzéseket, az éles környezetben befolyásolása nélkül.

[Itt részletesebben tájékozódhat](site-recovery-sql.md) az SQL Server védelméről.

## <a name="protect-sharepoint"></a>A SharePoint védelme
Az Azure Site Recovery szolgáltatással az alábbi módokon biztosíthatja a SharePoint-környezetek védelmét:

* Megszünteti hello kell, és a készenléti farm vész-helyreállítási költséges. A Site Recovery tooreplicate egy teljes farm (Web-, alkalmazás és az Adatbázisréteg) tooAzure vagy tooa másodlagos helyet használja.
* Leegyszerűsíti az alkalmazások üzembe helyezését és felügyeletét. Telepített frissítések toohello elsődleges helyet a rendszer automatikusan replikálja, és így elérhető feladatátvételét és helyreállítását a másodlagos helyek a farm követően. Hello felügyelet összetettségét, valamint a készenléti farm naprakészen tartásával kapcsolatos költségeket is csökkenti.
* Leegyszerűsíti a SharePoint-alkalmazások fejlesztését és tesztelését, mivel segítségével az éleshez hasonló, igény szerinti replikakörnyezet hozható létre a teszteléshez és a hibakereséshez.
* Egyszerűbbé teszi a átmenet toohello felhő Site Recovery toomigrate SharePoint központi telepítések tooAzure használatával.

[Itt részletesebben olvashat](site-recovery-sharepoint.md) a SharePoint védelméről.

## <a name="protect-dynamics-ax"></a>A Dynamics AX védelme
Az Azure Site Recovery szolgáltatással az alábbi módokon biztosíthatja a Dynamics AX ERP-megoldás védelmét:

* Az egész Dynamics AX-környezetnek (a webes és AOS-rétegeknek, az adatbázisrétegeknek, SharePoint) replikálásával tooAzure, vagy a másodlagos hely tooa.
* A Dynamics AX központi telepítések toohello Azure-felhőbe áttelepítését leegyszerűsítve.
* Leegyszerűsíti a Dynamics AX-alkalmazások fejlesztését és tesztelését, mivel segítségével az éleshez hasonló, igény szerinti másolat hozható létre a teszteléshez és a hibakereséshez.

[Itt részletesebben tájékozódhat](site-recovery-dynamicsax.md) a Dynamic AX védelméről.

## <a name="protect-rds"></a>A távoli asztali szolgáltatások védelme
Távoli asztali szolgáltatások (RDS) lehetővé teszik virtuális asztali infrastruktúra (VDI), a munkamenet-alapú asztali környezetek és alkalmazások, így a felhasználók toowork bármely részén. Az Azure Site Recovery segítségével:

* Felügyelt vagy nem kezelt készletezett virtuális asztalok tooa másodlagos helyre, és a távoli alkalmazásait és munkameneteit tooa másodlagos helyet vagy az Azure replikálni.
* A következőket replikálhatja:

| **RDS** | **Másodlagos hely replikálást a Hyper-V virtuális gépek tooa** | **Hyper-V virtuális gépek tooAzure replikálása** | **VMware virtuális gépek tooa másodlagos helyre replikálni.** | **VMware virtuális gépek tooAzure replikálása** | **Fizikai kiszolgálók replikálása tooa másodlagos helyen** | **Fizikai kiszolgálók tooAzure replikálása** |
| --- | --- | --- | --- | --- | --- | --- |
| **Készletezett virtuális asztal (nem kezelt)** |Igen |Nem |Igen |Nem |Igen |Nem |
| **Készletezett virtuális asztal (kezelt, UPD nélküli)** |Igen |Nem |Igen |Nem |Igen |Nem |
| **Távoli alkalmazások és asztali munkamenetek (UPD nélkül)** |Igen |Igen |Igen |Igen |Igen |Igen |

[Itt részletes tájékoztatást olvashat](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) az RDS védelméről.

## <a name="protect-exchange"></a>Az Exchange védelme
A Site Recovery szolgáltatással az alábbi módokon biztosíthatja az Exchange-környezetek védelmét:

* A kisebb Exchange üzemelő példányok, például önálló vagy az önálló kiszolgálók a Site Recovery is replikálásához és feladatátadásához tooAzure vagy tooa másodlagos helyen.
* Nagyobb méretű környezetek esetén a Site Recovery integrálható az Exchange DAG-kkal.
* Exchange dag-k hello vállalati Exchange vész-helyreállítási megoldás ajánlott.  Site Recovery helyreállítási terveiben tartalmazhatnak dag, tooorchestrate DAG feladatátvételi helyek között.

[További információk](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) az Exchange védelméről.

## <a name="protect-sap"></a>Az SAP védelme
A Site Recovery tooprotect az SAP üzemelő példányt, a következőképpen használhatja:

* Engedélyezi a helyszíni-t futtató összetevők tooAzure replikálása SAP NetWeaver és NetWeaver termelési alkalmazások védelmét.
* Engedélyezi Azure,-t futtató replikálása Azure-adatközpontban összetevők tooanother SAP NetWeaver és NetWeaver termelési alkalmazások védelmét.
* Leegyszerűsíti a áttelepülés a felhőbe, a Site Recovery toomigrate a SAP telepítési tooAzure.
* Leegyszerűsíti az SAP projektek frissítését, tesztelését és prototípuskészítését, mivel a segítségével egy igény szerinti éles klón hozható létre az SAP alkalmazások teszteléséhez.

[Itt részletesen tájékozódhat](site-recovery-sap.md) az SAP védelméről.

## <a name="protect-iis"></a>Az IIS védelme
A Site Recovery tooprotect az IIS telepítése a következőképpen használhatja:

Az Azure Site Recovery vész-helyreállítási replikálása hello kritikus fontosságú összetevők a környezet tooa cold távoli hely vagy a nyilvános felhők, például a Microsoft Azure biztosít. Hello webkiszolgáló és hello adatbázis hello virtuális gép replikált toohello helyreállítási hely folyamatban van, mert nincs követelmény toobackup konfigurációs fájlok vagy tanúsítványok külön-külön. alkalmazás-hozzárendelések hello, és kötések függ a környezeti változókat, amelyek a módosított post feladatátvételi hello vész-helyreállítási tervek integrálva parancsprogramok frissíthető. Virtuális gépek hello helyreállítási helyen csak egy feladatátvételi hello esemény a kerülnek sorra. Nem csak az Azure Site Recovery is segítséget nyújt hello end tooend feladatátvétel levezényléséhez, adja meg a következő képességek hello:

-   Alkalmazás-előkészítés hello leállítása és a virtuális gépek indítási hello különböző rétegek.
-   Parancsfájlok tooallow frissítés alkalmazásfüggőségek és kötések hozzáadása hello virtuális gépeken megkezdése után. hello parancsfájlokat is használt tooupdate hello DNS kiszolgáló toopoint toohello helyreállítási helyet.
-   Foglaljon le IP címek toovirtual gépek előtti feladatátvételt hello elsődleges és a helyreállítási hálózatok társításával, ezért a parancsfájlok, melyeket nem szükséges a frissített toobe post feladatátvételi.
-   Az egy kattintással feladatátvétel több webes alkalmazásokhoz, a hello webkiszolgálókon, így a hello esemény egy olyan vészhelyzet esetén a félreértések hello hatóköre nem képesek.
-   Képes tootest hello helyreállítási tervek a vész-Helyreállítási csukja elkülönített környezetben.

[Itt részletesen tájékozódhat](https://aka.ms/asr-iis) az IIS webfarm védelméről.

## <a name="protect-citrix-xenapp-and-xendesktop"></a>A Citrix XenApp és a XenDesktop védelme
A Site Recovery tooprotect a Citrix XenApp és XenDesktop központi telepítéseket használ, az alábbiak szerint:

* Védelem engedélyezése a Citrix XenApp és XenDesktop központi telepítéskor különböző telepítési replikálása hello réteges beleértve (AD DNS-kiszolgáló, az SQL adatbázis-kiszolgáló, Citrix kézbesítési vezérlő, kirakat kiszolgálót, XenApp főkiszolgáló (VDA), Citrix XenApp licenckiszolgáló) tooAzure.
* Áttelepülés a felhőbe, leegyszerűsítheti a Site Recovery toomigrate a Citrix XenApp és XenDesktop telepítési tooAzure.
* Leegyszerűsíti a Citrix XenApp-/XenDesktop-fejlesztést és -tesztelést, mivel segítségével az éleshez hasonló, igény szerinti másolat hozható létre az alkalmazások teszteléséhez és a hibakereséséhez.
* Ez a megoldás kizárólag a Windows Server operációs rendszer virtuális asztali környezeteire alkalmazható, az ügyfelek virtuális asztali környezetei esetében nem, mivel azok licencelése az Azure-ban még nem támogatott.
[Itt részletesen tájékozódhat](https://azure.microsoft.com/pricing/licensing-faq/) az ügyfél/kiszolgáló asztali környezeteinek Azure-ban történő licenceléséről.

[Itt részletesen tájékozódhat](site-recovery-citrix-xenapp-and-xendesktop.md) az üzemelő Citrix XenApp- és XenDesktop-példányok védelméről. Azt is megteheti, olvassa el a hello [a Citrix tanulmány](https://aka.ms/citrix-xenapp-xendesktop-with-asr) részletező hello azonos.

## <a name="next-steps"></a>Következő lépések
[Előfeltételek ellenőrzése](site-recovery-prereq.md)
