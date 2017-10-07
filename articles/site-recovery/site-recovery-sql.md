---
title: "az SQL Server és az Azure Site Recovery aaaReplicate alkalmazások |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooreplicate SQL Server az SQL Server vészhelyreállítási képességek az Azure Site Recovery segítségével."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: pratshar
ms.openlocfilehash: 99755f2cd2f7e924071f1e230ac4a0bda88f0a39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protect-sql-server-using-sql-server-disaster-recovery-and-azure-site-recovery"></a>SQL Server vészhelyreállítási és az Azure Site Recovery használata az SQL Server védelme

Ez a cikk ismerteti, hogyan tooprotect hello SQL Server háttér kombinációjából SQL Server üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási technológiákat, az alkalmazások és [Azure Site Recovery](site-recovery-overview.md).

Mielőtt elkezdené, győződjön meg arról megismerte az SQL Server katasztrófa utáni helyreállítás lehetőségekről, beleértve a Feladatátvételi fürtszolgáltatás, Always On rendelkezésre állási csoportok, az adatbázis-tükrözés és naplóküldésben.


## <a name="sql-server-deployments"></a>SQL Server-telepítések

Számos különféle munkaterheléshez alapjaként SQL Servert használja, és alkalmazások, például a SharePoint, a Dynamics és az SAP, tooimplement adatszolgáltatások integrálható.  SQL Server számos módon telepíthető:

* **Önálló SQL Server**: SQL Server és az összes adatbázis üzemeltetett (fizikai vagy virtuális) egy gépen. Amikor virtualizált, helyi magas rendelkezésre állású gazdagépen a fürtszolgáltatás szolgál. Vendég szint magas rendelkezésre állású nincs implementálva.
* **SQL Server feladatátvételi fürtszolgáltatási példányok (mindig az FCI)**: instanced megosztott lemezzel rendelkező SQL Server rendszert futtató két vagy több csomópont egy Windows feladatátvevő fürtben vannak konfigurálva. Ha egy csomópont leáll, hello fürt átveheti az SQL Server tooanother példány. A telepítő egy elsődleges helyen általánosan használt tooimplement magas rendelkezésre állású. A központi telepítés a nem sikertelen vagy ezen kívül az hello megosztott tárolási réteg elleni védelem érdekében. Egy megosztott lemez iSCSI, a fiber channel vagy a megosztott vhdx használatával valósítható meg.
* **SQL Always On rendelkezésre állási csoportok**: egy megosztott semmi-fürtöt, SQL Server-adatbázisok rendelkezésre állási csoportban, a szinkron replikáció és az automatikus feladatátvételre konfigurált a két vagy több csomópont állíthatók be.

 Ez a cikk a következő adatbázisok tooa távoli hely helyreállításához a natív SQL vész helyreállítási technológiák hello használja:

* SQL Always On rendelkezésre állási csoportok, tooprovide katasztrófa utáni helyreállítás az SQL Server 2012 vagy 2014 Enterprise kiadások.
* SQL adatbázis-tükrözés a magas biztonsági üzemmódot, az SQL Server Standard edition (bármilyen verzió), vagy az SQL Server 2008 R2.

## <a name="site-recovery-support"></a>Webhely-helyreállítási támogatás

### <a name="supported-scenarios"></a>Támogatott helyzetek
A Site Recovery megvédheti az SQL Server hello táblázatban foglaltak szerint.

**A forgatókönyv** | **másodlagos hely tooa** | **tooAzure**
--- | --- | ---
**Hyper-V** | Igen | Igen
**VMware** | Igen | Igen
**Fizikai kiszolgáló** | Igen | Igen

### <a name="supported-sql-server-versions"></a>Támogatott SQL Server-verziók
Ezek az SQL Server-verziók hello támogatott forgatókönyvek támogatottak:

* SQL Server 2016 Enterprise és Standard
* SQL Server 2014 Enterprise és Standard
* SQL Server 2012 Enterprise és Standard
* SQL Server 2008 R2 Enterprise és Standard

### <a name="supported-sql-server-integration"></a>Támogatott SQL Server-integráció

A Site Recovery integrálható natív SQL Server BCDR-technológiákkal hello táblázat, a vész-helyreállítási megoldást tooprovide foglalja össze.

**Funkció** | **Részletek** | **SQL Server** |
--- | --- | ---
**Always On rendelkezésre állási csoport** | Több különálló példány az SQL Server futtatása több csomóponttal rendelkező feladatátvevő fürtben.<br/><br/>Adatbázisok másolható (tükrözött) az SQL Server-példányokat, hogy nincs megosztott tárhely szükséges feladatátvevő csoportokba csoportosíthatók.<br/><br/>Itt katasztrófa utáni helyreállítás egy elsődleges hely és egy vagy több másodlagos hely között. Két csomópontot is kell beállítani egy megosztott semmi szinkron replikáció és automatikus feladatátvételt egy rendelkezésre állási csoportban konfigurált SQL Server-adatbázisok fürt. | Az SQL Server 2014 & 2012 Enterprise edition
**Feladatátvételi fürtszolgáltatás (mindig az FCI)** | SQL Server kihasználja a Windows feladatátvételi fürtszolgáltatás magas rendelkezésre állás, a helyszíni SQL Server-munkaterhelések.<br/><br/>SQL Server-példány fut a megosztott lemezek csomópontja feladatátvevő fürtben van konfigurálva. Ha a példány nem működik a hello fürt átadja egy toodifferent.<br/><br/>hello fürt elleni nem sikertelen vagy üzemszünet korlátozza a megosztott tárolóeszköz. hello megosztott lemez végrehajtható, szálcsatorna, iSCSI vagy a megosztott vhdx-fájlokat. | SQL Server Enterprise kiadása<br/><br/>SQL Server Standard edition (csak korlátozott tootwo csomópontok)
**Az adatbázis-tükrözés (magas biztonsági mód)** | Egy önálló adatbázis tooa másodlagos példányt védi. Elérhető mindkét magas biztonsági (szinkron) és nagy teljesítményű (aszinkron) replikációs mód. A feladatátvevő fürtök nem igényel. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise minden kiadás
**Önálló SQL-kiszolgáló** | hello SQL Server és az adatbázis egy kiszolgálón (fizikai vagy virtuális) üzemelteti. Ha hello kiszolgáló virtuális állomás fürtszolgáltatás magas rendelkezésre állású használható. Nincs vendégszintű magas rendelkezésre állású. | Enterprise vagy Standard edition

## <a name="deployment-recommendations"></a>Telepítési javaslatok

A táblázat összefoglalja, az SQL Server BCDR-technológiákkal integrálása a Site Recovery javaslatok.

| **Verzió** | **Kiadás** | **Üzembe helyezés** | **Helyszíni tooon helyszínen** | **Helyszíni tooAzure** |
| --- | --- | --- | --- | --- |
| SQL Server 2014 vagy 2012 |Enterprise |Feladatátvevőfürt-példány |Always On rendelkezésre állási csoportok |Always On rendelkezésre állási csoportok |
|| Enterprise |Always On rendelkezésre állási csoportokat magas rendelkezésre álláshoz |Always On rendelkezésre állási csoportok |Always On rendelkezésre állási csoportok | |
|| Standard |Feladatátvevőfürt-példány (FCI) |A helyi tükör helyreállítási helyreplikálásának |A helyi tükör helyreállítási helyreplikálásának | |
|| Enterprise vagy Standard |Különálló |Helyreállítási helyreplikálásának |Helyreállítási helyreplikálásának | |
| SQL Server 2008 R2 vagy 2008 |Enterprise vagy Standard |Feladatátvevőfürt-példány (FCI) |A helyi tükör helyreállítási helyreplikálásának |A helyi tükör helyreállítási helyreplikálásának |
|| Enterprise vagy Standard |Különálló |Helyreállítási helyreplikálásának |Helyreállítási helyreplikálásának | |
| SQL Server (bármilyen verzió) |Enterprise vagy Standard |Feladatátvevőfürt-példány - DTC-alkalmazás |Helyreállítási helyreplikálásának |Nem támogatott |

## <a name="deployment-prerequisites"></a>Üzembehelyezési előfeltételek

* Egy helyi SQL Server-telepítéséhez, egy SQL Server támogatott verzióját futtatja. Általában szükség az Active Directory az SQL-kiszolgáló.
* hello követelményei hello toodeploy kívánt forgatókönyvben. További információ a támogatási követelmények [replikációs tooAzure](site-recovery-support-matrix-to-azure.md) és [helyszíni](site-recovery-support-matrix.md), és [telepítésének előfeltételei](site-recovery-prereq.md).
* az Azure, futtatási hello helyreállítási tooset [Azure virtuális gép Readiness Assessment](http://www.microsoft.com/download/details.aspx?id=40898) eszköz az SQL Server virtuális gépeken toomake meg arról, hogy a kompatibilis Azure és a Site Recovery fontosságúak.

## <a name="set-up-active-directory"></a>Az Active Directory beállítása

Active Directory hello másodlagos helyreállítási hely, az SQL Server toorun megfelelően beállítva.

* **Kis vállalati**– néhány alkalmazások és a helyszíni hely hello egyetlen tartományvezérlővel rendelkezik, ha azt szeretné, toofail keresztül hello teljes webhelyet, javasolt a Site Recovery replikációs tooreplicate hello tartományvezérlő használata toohello másodlagos adatközpontba, vagy tooAzure.
* **Közepes toolarge vállalati**– Ha nagyszámú alkalmazásokat, egy Active Directory-erdőben van, és azt szeretné, hogy át alkalmazás vagy munkaterhelés toofail, ajánlott hello másodlagos adatközpontba, vagy a tartományvezérlő beállítása Azure. Ha az Always On rendelkezésre állási csoportok toorecover tooa távoli hely használata esetén ajánlott állít be egy másik tartományvezérlő hello másodlagos helyen vagy az Azure, toouse helyre hello SQL Server-példányhoz.

hello jelen cikkben lévő utasítások feltételezik, hogy egy tartományvezérlő legyen hello másodlagos helyen. [További](site-recovery-active-directory.md) az Active Directory, a Site Recovery védelméről.


## <a name="integrate-with-sql-server-always-on-for-replication-tooazure"></a>Az SQL Server Always On replikációs tooAzure integrálása

Mi szüksége toodo:

1. Parancsfájlok importálnia kell az Azure Automation-fiók. Hello parancsfájlok toofailover SQL rendelkezésre állási csoport tartalmazza a egy [Resource Manager virtuális gép](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAG.ps1) és egy [klasszikus virtuális gép](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAGClassic.ps1).

    [![TooAzure telepítése](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)


1. Adja hozzá az ASR-SQL-FailoverAG előtti műveletként hello első csoport hello helyreállítási terv.

1. Útmutatás alapján hello hello parancsfájl toocreate egy automatizálási változó tooprovide hello neve elérhető hello rendelkezésre állási csoportok.

### <a name="steps-toodo-a-test-failover"></a>Feladatátvételi teszt lépéseket toodo

SQL Always On nem natív módon támogatja a feladatátvételi tesztet. Ezért ajánlott hello következő:

1. Állítson be [Azure biztonsági mentés](../backup/backup-azure-vms.md) hello virtuális gépen, amelyen hello rendelkezésre állási csoport replika Azure-ban.

1. Ahhoz, hogy kiváltsa hello helyreállítási terv teszt feladatátvétele, hello készült biztonsági másolatok hello előző lépésben hello virtuális gép helyreállítása.

    ![Állítsa vissza az Azure biztonsági másolatból ](./media/site-recovery-sql/restore-from-backup.png)

1. [A kvórum](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum#PowerShellProcedure) hello virtuális gép biztonsági másolatból állítottak vissza.

1. Frissítse az IP-figyelő hello tooan hello feladatátvételi teszt hálózatában elérhető IP-címe.

    ![Frissítse a figyelő IP](./media/site-recovery-sql/update-listener-ip.png)

1. Figyelő online állapotba.

    ![Figyelő kapcsolása](./media/site-recovery-sql/bring-listener-online.png)

1. A terheléselosztó előtérbeli IP készlet megfelelő tooeach rendelkezésre állási csoport figyelőjének alatt hozott létre egy IP-cím és a hozzáadott hello háttérkészlet hello SQL-virtuális gép létrehozása

     ![Terheléselosztó - előtér-IP-címkészlet létrehozása ](./media/site-recovery-sql/create-load-balancer1.png)

    ![Terheléselosztó - háttérkészlet létrehozása ](./media/site-recovery-sql/create-load-balancer2.png)

1. Végezzen hello helyreállítási terv feladatátvételi tesztet.

### <a name="steps-toodo-a-failover"></a>A feladatátvétel lépéseket toodo

Hozzáadását követően hello parancsfájl hello helyreállítási terv és ellenőrzött hello helyreállítási terv feladatátvételi teszt elvégzésével, hello helyreállítási terv feladatátvételi teheti meg.


## <a name="integrate-with-sql-server-always-on-for-replication-tooa-secondary-on-premises-site"></a>Integrálható az SQL Server Always On replikációs tooa másodlagos helyszíni helyre

Ha hello SQL Server rendelkezésre állási csoportokat használ a magas rendelkezésre állású (vagy egy FCI), azt javasoljuk, rendelkezésre állási csoportokat, valamint a hello helyreállítási helyen. Vegye figyelembe, hogy ez vonatkozik, amelyek nem használják az elosztott tranzakciók tooapps.

1. [Adatbázisok konfigurálása](https://msdn.microsoft.com/library/hh213078.aspx) rendelkezésre állási csoportokba.
1. Virtuális hálózat létrehozása a hello másodlagos helyen.
1. A pont-pont VPN-kapcsolat állítható be hello virtuális hálózat, és hello elsődleges hely között.
1. Hozzon létre egy virtuális gépet hello helyreállítási helyen, és az SQL Server telepítését.
1. Hello meglévő Always On rendelkezésre állási csoportok toohello bővítése új SQL Server virtuális gép. Az SQL Server-példány beállítása egy aszinkron replika másolatot.
1. Egy rendelkezésre állási csoport figyelőjének létrehozása vagy frissítése hello meglévő figyelő tooinclude hello aszinkron replika virtuális géphez.
1. Győződjön meg arról, hogy hello alkalmazás farm hello figyelő használatára van beállítva. Ha a szolgáltatás-telepítés használatával hello adatbázis-kiszolgáló nevét, frissítse toouse hello figyelő, így nem kell tooreconfigure azt hello feladatátvételt követően.

Az elosztott tranzakciókat használó alkalmazásokat, azt javasoljuk, a Site Recovery telepítése [VMware vagy fizikai kiszolgáló helyek közötti replikálás](site-recovery-vmware-to-vmware.md).

### <a name="recovery-plan-considerations"></a>Helyreállítási terv kapcsolatos szempontok
1. Adja hozzá a minta parancsprogram toohello VMM-könyvtárban, hello elsődleges és másodlagos helyen.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

1. A helyreállítási terv hello alkalmazás létrehozásakor előtti művelet tooGroup-1 parancsfájlalapú lépés hozzáadása, amely hívja meg hello parancsfájl toofail keresztül a rendelkezésre állási csoportokat.

## <a name="protect-a-standalone-sql-server"></a>Különálló SQL Server védelme

Ebben a forgatókönyvben azt javasoljuk, hogy használja-e a Site Recovery replikációs tooprotect hello SQL Server-számítógépen. hello szövegéről függ, hogy az SQL Server egy virtuális gép vagy fizikai kiszolgálók, és azt szeretné, hogy tooreplicate tooAzure vagy egy másodlagos helyszíni hely. További tudnivalók [Site Recovery forgatókönyvek](site-recovery-overview.md).

## <a name="protect-a-sql-server-cluster-standard-editionwindows-server-2008-r2"></a>(Standard edition és Windows Server 2008 R2) SQL Server-fürt védelme

SQL Server Standard edition vagy SQL Server 2008 R2 rendszerű fürtökön javasoljuk, használjon a Site Recovery replikációs tooprotect SQL Server.

### <a name="on-premises-tooon-premises"></a>A helyszíni tooon helyszíni

* Ha hello alkalmazása használja-e az elosztott tranzakciók ajánlott telepít [SAN-replikálással a Site Recovery](site-recovery-vmm-san.md) Hyper-V-környezethez vagy [VMware vagy fizikai kiszolgáló tooVMware](site-recovery-vmware-to-vmware.md) VMware környezetben.
* Nem DTC-alkalmazások esetében használják fent megközelítés toorecover hello fürt hello egy önálló kiszolgáló, ami egy helyi magas biztonsági adatbázis-tükrözés.

### <a name="on-premises-tooazure"></a>A helyszíni tooAzure

A Site Recovery nem biztosít a Vendég Fürtözési támogatás tooAzure replikálása esetén. SQL Server is nem biztosít egy alacsony költségű vész-helyreállítási megoldást a Standard kiadásban. Ebben a forgatókönyvben javasoljuk, hogy hello a helyszíni SQL Server fürt tooa különálló SQL kiszolgáló védelmére, és végezze el a helyreállítást a Azure-ban.

1. További önálló SQL Server-példány a hello helyszíni helyen konfigurálja.
1. Konfigurálása hello példány tooserve tükör hello adatbázisok azt szeretné, hogy tooprotect. Konfigurálja a tükrözés magas biztonsági módban.
1. Hello helyszíni hely, a Site Recovery konfigurálása ([Hyper-V](site-recovery-hyper-v-site-to-azure.md) vagy [VMware virtuális gépek/fizikai kiszolgálók)](site-recovery-vmware-to-azure-classic.md).
1. A Site Recovery replikációs tooreplicate hello új SQL Server példány tooAzure használja. Mivel tükör magas biztonsági másolatot, hello elsődleges fürttel szinkronizálja, de a Site Recovery-replikációval replikált tooAzure lesz.


![Standard fürt](./media/site-recovery-sql/standalone-cluster-local.png)

### <a name="failback-considerations"></a>Feladat-visszavétel kapcsolatos szempontok

Az SQL Server Standard-fürtök esetén a feladat-visszavétel nem tervezett feladatátvétel után az SQL server biztonsági másolat és a visszaállítási hello tükör példány toohello eredeti fürtből, a tükör hello reestablishment szükséges.

## <a name="next-steps"></a>Következő lépések
[További](site-recovery-components.md) Site Recovery architektúrájáról kapcsolatban.
