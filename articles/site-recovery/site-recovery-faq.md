---
title: "Az Azure Site Recovery: Gyakori kérdések |} Microsoft Docs"
description: "Ez a cikk ismerteti, amelyek a népszerű kérdések Azure Site Recoveryvel kapcsolatos."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5cdc4bcd-b4fe-48c7-8be1-1db39bd9c078
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/22/2017
ms.author: raynew
ms.openlocfilehash: 6d0bd2475466e5745e1f084bd2267d954d624ebd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure Site Recovery: gyakori kérdések (GYIK)
A cikk az Azure Site Recovery kapcsolatos gyakran ismételt kérdések tartalmaz. Ha kérdése van a cikk elolvasása után, az fel őket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).

## <a name="general"></a>Általános kérdések
### <a name="what-does-site-recovery-do"></a>Mire való a Site Recovery?
A Site Recovery funkciója hozzájárul tooyour üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégiát, levezényli és automatizálja a régiókban, a helyszíni virtuális gépek és fizikai kiszolgálók tooAzure között Azure virtuális gépek replikálását, és a helyszíni gépeket tooa másodlagos adatközpont. [További információk](site-recovery-overview.md).

### <a name="what-can-site-recovery-protect"></a>Mi is védeni a Site Recovery?
* **Azure virtuális gépek**: a Site Recovery bármilyen a támogatott Azure virtuális gép számítási feladatot képes replikálni.
* **Hyper-V virtuális gépek**: tudja védeni a Site Recovery bármilyen munkaterhelést egy Hyper-V virtuális gépen.
* **Fizikai kiszolgálók**: a Site Recovery windowsos vagy Linuxos fizikai kiszolgálókat védhet.
* **VMware virtuális gépek**: tudja védeni a Site Recovery bármilyen VMware virtuális gép futó munkaterhelések.

### <a name="does-site-recovery-support-hello-azure-resource-manager-model"></a>Támogatja a Site Recovery hello Azure Resource Manager modellt?
A Site Recovery hello Azure portál, amely támogatja az erőforrás-kezelő érhető el. A klasszikus Azure portálon hello a Site Recovery támogatja a hagyományos központi telepítéseket. Nem hozható létre új tárolók hello a klasszikus portálon, és új funkciók nem támogatottak.

### <a name="can-i-replicate-azure-vms"></a>Azure virtuális gépek képes replikálni?
Igen, replikálhatja támogatott Azure virtuális gépek Azure-régiók között. [További információk](site-recovery-azure-to-azure.md).

### <a name="what-do-i-need-in-hyper-v-tooorchestrate-replication-with-site-recovery"></a>Mi szükséges a Hyper-V tooorchestrate replikáció a Site Recoveryvel?
Hello Hyper-V gazdakiszolgáló teendők függ hello környezet-forgatókönyv. Tekintse meg a hello a Hyper-V előfeltételei:

* [Hyper-V virtuális gépek (VMM nélkül) tooAzure replikálása](site-recovery-hyper-v-site-to-azure.md)
* [Hyper-V virtuális gépek (VMM) tooAzure replikálása](site-recovery-vmm-to-azure.md)
* [Hyper-V virtuális gépek tooa másodlagos adatközpontba replikálása](site-recovery-vmm-to-vmm.md)
* Ha másodlagos tooa replikál datacenter olvassa [Hyper-V virtuális gépek esetén a vendég operációs rendszerek támogatott](https://technet.microsoft.com/library/mt126277.aspx).
* Ha tooAzure replikál, a Site Recovery támogatja-e minden hello vendég operációs rendszerek, amelyek [használható az Azure-](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Képes a virtuális gépek védelme Ha Hyper-V ügyfél operációs rendszeren fut?
Nem. A virtuális gépeknek egy támogatott Windows kiszolgáló gépen futó Hyper-V gazdakiszolgálón kell lenniük. Ha tooprotect van szüksége egy ügyfélszámítógép meg lehetett replikálhatja azt fizikai gépként túl[Azure](site-recovery-vmware-to-azure.md) vagy egy [másodlagos adatközpontba](site-recovery-vmware-to-vmware.md).

### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Milyen számítási feladatokat tud védeni a Site Recovery?
A Site Recovery tooprotect támogatott virtuális és fizikai kiszolgálókon futó legtöbb munkaterhelések használhatja. A Site Recovery támogatja az alkalmazástudatos replikációt, így az alkalmazások telepíthetők tooan működőképes állapotban helyre. Integrálható a Microsoft-alkalmazások, például a SharePoint, Exchange, Dynamics, SQL Server és Active Directory, és szorosan együttműködik olyan vezető szállítókkal, beleértve az Oracle, SAP, IBM és Red Hat. [További információk](site-recovery-workload.md) a számítási feladatok védelméről.

### <a name="do-hyper-v-hosts-need-toobe-in-vmm-clouds"></a>Igényelnek a Hyper-V-gazdagépek a VMM-felhőkben toobe?
Ha tooreplicate tooa másodlagos adatközpontba, akkor a Hyper-V virtuális gépek kell lennie a Hyper-V gazdagépek a VMM-felhőben lévő kiszolgálók. Ha azt szeretné, hogy tooreplicate tooAzure, majd replikálhatja virtuális gépek a Hyper-V gazdakiszolgálókra vagy a VMM-felhő nélkül. [További információk](site-recovery-hyper-v-site-to-azure.md).

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Üzembe helyezhetem VMM-mel a Site Recovery-t, ha csak egy VMM-kiszolgálóm van?

Igen. Replikálhat virtuális gépeket a Hyper-V kiszolgálók, a VMM-felhő tooAzure hello, vagy replikálhatja hello található VMM-felhők közötti azonos kiszolgálón. A helyszíni tooon replikáció javasoljuk, hogy rendelkezik-e a VMM-kiszolgáló mindkét hello elsődleges és másodlagos hely.  

### <a name="what-physical-servers-can-i-protect"></a>Milyen fizikai kiszolgálókat lehet védeni?
A Windows és Linux tooAzure vagy tooa másodlagos helyet futtató fizikai kiszolgálók replikálhatja. [További tudnivalók](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) operációs rendszerre vonatkozó követelményeket.  hello ugyanazok a követelmények érvényesek, hogy a fizikai kiszolgálók tooAzure vagy tooa másodlagos helyre replikál.


Vegye figyelembe, hogy fizikai kiszolgálók virtuális gépként fognak futni az Azure-ban, ha a helyszíni kiszolgáló leáll. Feladat-visszavétel tooan helyszíni fizikai kiszolgáló jelenleg nem támogatott. Védett, fizikai gépek esetén csak a feladat-visszavétel tooa VMware rendszerű virtuális gép is.

### <a name="what-vmware-vms-can-i-protect"></a>Milyen VMware virtuális gépek védhetők?

VMware virtuális gépek tooprotect szüksége lesz egy vSphere hipervizorra, és a VMware-eszközök futó virtuális gépek. Azt javasoljuk, hogy rendelkezik-e a VMware vCenter server toomanage hello hipervizorok. [További](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) replikálásához a VMware-kiszolgálók és virtuális gépek tooAzure vagy tooa másodlagos hely részletes követelményeiről.


### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Elérhető a vészhelyreállítás a fiókirodák számára a Site Recoveryvel?
Igen. A fiókirodákban a Site Recovery tooorchestrate replikációs és feladatátvételi használatánál, egy központi helyen egy egységes vezénylési és a nézet az összes a fiókiroda számítási feladatait fog kapni. Könnyen intézheti a feladatátvételt, és a vészhelyreállítást az összes ágak a központi irodából hello ágak meglátogatása nélkül.

## <a name="pricing"></a>Díjszabás

### <a name="what-charges-do-i-incur-while-using-azure-site-recovery"></a>Milyen költségekkel tegye járnak Azure Site Recovery használata során?
A Site Recovery használata esetén függő díj terheli hello Site Recovery licenc, az Azure storage, storage-tranzakció és a kimenő adatforgalom. [További információk](https://azure.microsoft.com/pricing/details/site-recovery).

hello Site Recovery licencre van egy védett példány, ha egy példány a virtuális gép vagy fizikai kiszolgálók.

- Ha egy virtuális gép lemez tooa standard szintű tárfiók replikálja, hello az Azure storage kell fizetni hello tárhelyhasználatot szolgál. Például ha hello forrás lemez mérete 1 TB-os és 400 GB használja, a Site Recovery egy 1 TB méretű virtuális merevlemez létrehozása az Azure-ban, de felszámított hello tárolási 400 GB (és a replikálási naplókhoz felhasznált tárterület hello mennyisége).
- Ha egy virtuális gép lemez tooa prémium szintű tárfiók replikálja, hello az Azure storage kell fizetni van kiépítve hello tároló méretét, a prémium szintű tároló lemez használata a legközelebbi hello kerekítve. Például ha hello adatforrás lemez mérete 50 GB, a Site Recovery létrehoz egy 50 GB lemezterület az Azure-ban, és Azure társít a prémium szintű tároló lemez (P10) legközelebbi toohello.  Költségszámítás P10, nem pedig hello 50 GB-os lemez méretét.  [További információk](https://aka.ms/premium-storage-pricing).  Prémium szintű storage használata, a standard szintű tárfiók replikációs naplózási is szükség, és ezek a naplók használt szabványos tárhely hello mennyiségét is lesz számlázva.
- Lemez vége: feladatátvételi teszt vagy egy feladatátvételi jönnek létre. Hello replikációs állapotban tárolási költségek "oldalakra vonatkozó blob és lemez" hello kategóriához tartozó szerint hello [tárolási árképzési Számológép](https://azure.microsoft.com/en-in/pricing/calculator/) merülnek fel. Ezeket a díjakat hello tárolási típusa premium/standard és hello adatredundanciát alapuló írja be a - LRS, GRS RA-GRS stb.
- Ha hello beállítás toouse által kezelt lemezeken a feladatátvétel van kiválasztva, [felügyelt lemezek díjak](https://azure.microsoft.com/en-in/pricing/details/managed-disks/) feladatátvételi és tesztelési célú feladatátvétel után alkalmazza. A felügyelt lemezek díjak nem vonatkoznak a replikáció során.
- Ha hello beállítás felügyelt toouse lemezek feladatátvétel nincs bejelölve, tárolási szerint hello ki elszámolás "oldalakra vonatkozó blob és lemez" hello kategóriához tartozó [tárolási árképzési Számológép](https://azure.microsoft.com/en-in/pricing/calculator/) merülnek fel a feladatátvételt követően. Ezeket a díjakat hello tárolási típusa premium/standard és hello adatredundanciát alapuló írja be a - LRS, GRS RA-GRS stb.
- Storage-tranzakció stabil állapot replikáció során, és a virtuális gép rendszeres műveletek van szó, a feladatátvétel után, vagy a feladatátvételi teszt. De ezek költségek elhanyagolhatóak.

Feladatátvételi teszt, ahol hello VM, a tárolás, a kimenő forgalom és a tárolási tranzakciók költségek alkalmazandó során költségek is merülnek fel.



## <a name="security"></a>Biztonság
### <a name="is-replication-data-sent-toohello-site-recovery-service"></a>Replikációs adatok küldött toohello Site Recovery szolgáltatás?
Nem, a Site Recovery nem gyűjt replikációs adatokat, és nem rendelkezik semmilyen információval, hogy a virtuális gépek és fizikai kiszolgálókon futó tartalomról.
A replikációs adatcsere a helyszíni Hyper-V gazdagépek, a VMware hipervizorok vagy fizikai kiszolgálók és az Azure tárolási szolgáltatás között történik. A Site Recovery már nem képes toointercept adatok. Csak hello metaadat szükséges tooorchestrate replikációs és feladatátvételi toohello Site Recovery szolgáltatás küldi el.  

A Site Recovery ISO 27001:2013, 27018, HIPAA, DPA hitelesített, de a SOC2 és FedRAMP JAB minősítés hello folyamatban van.

### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-hello-same-geographic-region-can-site-recovery-help-us"></a>Megfelelőségi okokból, még akkor is a helyi metaadatok hello belül kell maradnia ugyanabban a földrajzi régióban. A Site Recovery segíthet?
Igen. A Site Recovery-tárolóban egy régióban létrehozásakor gondoskodunk róla, hogy minden metaadatot, amely azt kell tooenable és replikációra és feladatátvételi adott régión belül marad a földrajzi határ.

### <a name="does-site-recovery-encrypt-replication"></a>A Site Recovery titkosítja a replikációt?
A virtuális gépek és fizikai kiszolgálók a helyszíni helyek titkosítási az átvitel közötti replikálása esetén támogatott. A virtuális gépek és fizikai kiszolgálók replikálása tooAzure, mindkét titkosítási az átvitel és [titkosítási nyugalmi (az Azure-ban)](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) támogatottak.

## <a name="replication"></a>Replikáció

### <a name="can-i-replicate-over-a-site-to-site-vpn-tooazure"></a>Replikálhatok a pont-pont VPN tooAzure keresztül?
Az Azure Site Recovery replikálja az adatokat tooan Azure storage-fiókok, egy nyilvános végpontot keresztül. Replikációs nem található a pont-pont VPN-kapcsolaton keresztül. A telephelyek közötti VPN, létrehozhat egy Azure virtuális hálózatra. Ez nem zavarja a Site Recovery replikációs.

### <a name="can-i-use-expressroute-tooreplicate-virtual-machines-tooazure"></a>Használható ExpressRoute tooreplicate virtuális gépek tooAzure?
Igen, a ExpressRoute használt tooreplicate virtuális gépek tooAzure lehet. Az Azure Site Recovery replikálja az adatokat tooan Azure Storage-fiók egy nyilvános végpontot keresztül. Be kell tooset [nyilvános társviszony](../expressroute/expressroute-circuit-peerings.md#public-peering) toouse ExpressRoute a Site Recovery-replikáció. Miután hello virtuális gépek Azure-beli virtuális hálózat tooan rendelkezik lett átvenni elérheti őket hello segítségével [magánhálózati társviszony-létesítés](../expressroute/expressroute-circuit-peerings.md#private-peering) hello Azure-beli virtuális hálózat telepítése.

### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-tooazure"></a>Van valamilyen előfeltétele a virtuális gépek tooAzure replikálására?
Virtuális gépek tooreplicate tooAzure kívánt meg kell felelnie [Azure-követelményeknek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

Az Azure felhasználói fiókot kell toohave bizonyos [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) egy új virtuális gép tooAzure tooenable replikációját.

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-tooazure"></a>Replikálhatja a Hyper-V generációs 2 virtuális gépek tooAzure?
Igen. A Site Recovery a feladatátvétel során a 2. generációs toogeneration 1 alakítja. A feladat-visszavétel hello gép, átalakított hátsó toogeneration 2. [További információk](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-tooazure-how-do-i-pay-for-azure-vms"></a>Hogyan kell fizetni az Azure virtuális gépek tooAzure replikálhassak?
A szokásos replikáció esetén adatok replikált toogeo redundáns az Azure storage és a felesleges toopay Azure IaaS virtuális gép díjakért jelentős előnyt biztosít. Futtatásakor egy feladatátvételi tooAzure, a Site Recovery automatikusan létrehoz az Azure infrastruktúra-szolgáltatási virtuális gépeket, és azt követően meg fogjuk számlázni hello az Azure-ban felhasznált számítási erőforrásokat.

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>Automatizálható a Site Recovery forgatókönyvek az SDK-val?
Igen. Automatizálható a Site Recovery munkafolyamatainak hello Rest API-t, a PowerShell vagy a hello Azure SDK-t. A PowerShell használatával a Site Recovery üzembe helyezésekor jelenleg támogatott esetek:

* [Hyper-V virtuális gépek replikálása az VMMs felhők tooAzure PowerShell erőforrás-kezelő](site-recovery-vmm-to-azure-powershell-resource-manager.md)
* [Hyper-V virtuális gépek replikálása nélkül VMM tooAzure PowerShell erőforrás-kezelő](site-recovery-deploy-with-powershell-resource-manager.md)

### <a name="if-i-replicate-tooazure-what-kind-of-storage-account-do-i-need"></a>TooAzure replikálhassak milyen típusú tárfiókra van szükségem?
* **A klasszikus Azure portálon**: Ha a Site Recovery a klasszikus Azure portálon hello telepít, szüksége lesz egy [standard georedundáns tárfiókot](../storage/common/storage-redundancy.md#geo-redundant-storage). Prémium szintű Storage jelenleg nem használható. hello fióknak kell lennie a hello és hello Site Recovery-tárolónak ugyanabban a régióban.
* **Azure-portálon**: a hello Azure-portál telepítése esetén a Site Recovery, egy LRS- vagy GRS-tárfiókra lesz szüksége. Georedundáns javasoljuk, hogy az adatok is lehetséges, ha egy regionális kimaradás során, vagy ha hello elsődleges régió nem állíthatók vissza. hello fióknak kell lennie a hello hello és ugyanabban a régióban Recovery Services-tároló. Prémium szintű storage mostantól támogatott a VMware virtuális gép, a Hyper-V virtuális gép és a fizikai kiszolgáló replikálás, a Site Recovery hello Azure-portálon való telepítésekor.

### <a name="how-often-can-i-replicate-data"></a>Milyen gyakran replikálhatom az adatokat?
* **Hyper-V:** Hyper-V virtuális gépek (kivéve a prémium szintű storage) 30 másodperces, 5 percenként vagy 15 percenként lehet replikálni. Ha beállítása SAN-replikálás akkor szinkron replikálása.
* **VMware és fizikai kiszolgálók:** a replikáció gyakoriságának ezeknél nincs jelentősége. Replikáció folyamatos.

### <a name="can-i-extend-replication-from-existing-recovery-site-tooanother-tertiary-site"></a>Kiterjesztheti a replikálást a meglévő helyreállítási pont tooanother harmadlagos pont?
A kiterjesztett vagy láncolt replikáció nem támogatott. Ez a szolgáltatás kérése [visszajelzési fórumon](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).

### <a name="can-i-do-an-offline-replication-hello-first-time-i-replicate-tooazure"></a>Feladatokat lehet elvégezni egy offline replikációt hello amikor első alkalommal replikálok tooAzure?
Ez a funkció nem támogatott. Ez a szolgáltatás hello kérelem [visszajelzési fórumon](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-specific-disks-from-replication"></a>Kizárhatok a replikációból bizonyos lemezeket?
Ez akkor támogatott, ha Ön [VMware virtuális gépek és a Hyper-V virtuális gépek replikálása](site-recovery-exclude-disk.md) tooAzure hello Azure-portál használatával.

### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Replikálhatok dinamikus lemezzel rendelkező virtuális gépek?
A dinamikus lemezek Hyper-V virtuális gépek replikálása esetén támogatottak. Ezek is támogatottak, ha VMware virtuális gépek és fizikai gépek tooAzure replikálása. hello operációs rendszert tároló lemezének alaplemeznek kell lennie.

### <a name="can-i-add-a-new-machine-tooan-existing-replication-group"></a>Adhat hozzá egy új gépet tooan meglévő replikációs csoport?
Új gépek tooexisting replikációs csoportok hozzáadása esetén támogatott. toodo Igen, válassza ki a hello replikációs csoporthoz ("Replikált elemek" paneljéről), és kattintson a jobb gombbal/kiválasztani helyi menü hello replikációs csoporton, és hello megfelelő beállítást.

![Tooreplication csoport hozzáadása](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Szabályozhatom a Hyper-V replikációs forgalom sávszélességét?
Igen. További tudnivalók hello telepítési cikkek sávszélesség-szabályozás:

* [VMware virtuális gépek és fizikai kiszolgálók replikálása kapacitástervezését](site-recovery-plan-capacity-vmware.md)
* [A Hyper-V virtuális gépek VMM-felhőkben replikálására kapacitástervezése](site-recovery-vmm-to-azure.md#capacity-planning)
* [A Hyper-V virtuális gépek VMM nélkül replikálására kapacitástervezése](site-recovery-hyper-v-site-to-azure.md)

## <a name="failover"></a>Feladatátvétel
### <a name="if-im-failing-over-tooazure-how-do-i-access-hello-azure-virtual-machines-after-failover"></a>Ha I vagyok feladatátadás tooAzure, hogyan férhetek hello Azure virtuális gépek a feladatátvételt követően?
Hello Azure virtuális gépeket biztonságos internetkapcsolaton keresztül, pont-pont VPN- vagy Azure ExpressRoute keresztül érheti el. Több minden a rendelés tooconnect tooprepare lesz szüksége. [További információ](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)


### <a name="if-i-fail-over-tooazure-how-does-azure-make-sure-my-data-is-resilient"></a>Ha I tooAzure hogyan Azure győződjön meg arról, hogy a feladatátvétel rugalmas az adataimat?
Az Azure-t hibatűrőnek terveztük. A Site Recovery már fejthetők vissza, a feladatátvételi tooa másodlagos Azure adatközpontba, hello Azure SLA-t, ha hello kell merül fel. Ha ez történik, biztosítjuk, hogy a metaadatok és tárolók lépje hello ugyanabban a földrajzi régióban, hogy a tároló számára kiválasztott.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Ha két adatközpont között végzek, mi történik, ha az elsődleges adatközpont esetleges váratlan leállásakor?
Egy nem tervezett feladatátvételt a másodlagos hely hello indíthat el. A Site Recovery nem követeli meg a hello elsődleges hely tooperform hello feladatátvételi.

### <a name="is-failover-automatic"></a>Automatikus a feladatátvétel?
A feladatátvétel nem automatikus. Feladatátvételt egyetlen kattintással hello portálon, vagy használhat [Site Recovery PowerShell](/powershell/module/azurerm.siterecovery) tootrigger a feladatátvételt. Sikertelen vissza nem egy egyszerű műveletek hello Site Recovery portálon.

tooautomate használhatja a helyszíni Orchestratort vagy az Operations Manager toodetect virtuális gép hibát, és ezután eseményindító hello feladatátvételi használatával hello SDK.

* [További](site-recovery-create-recovery-plans.md) helyreállítási tervek.
* [További](site-recovery-failover.md) feladatátvétellel kapcsolatos.
* [További](site-recovery-failback-azure-to-vmware.md) hibás kapcsolatos biztonsági másolatot a VMware virtuális gépek és fizikai kiszolgálók

### <a name="if-my-on-premises-host-is-not-responding-or-crashed-can-i-failover-back-tooa-different-host"></a>Ha a helyi gazdagép nem válaszol vagy lefagyott, lehetőségeket feladatátvételi hátsó tooa másik gazdagépen?
Igen, az hello másik helyre helyreállítási toofailback tooa másik gazdagépen az Azure-ból. További információk a hello-beállítások az alábbi hivatkozások hello VMware és a Hyper-v virtuális gépek.

* [A VMware virtuális gépek](site-recovery-how-to-failback-azure-to-vmware.md#fail-back-to-the-original-or-alternate-location)
* [A Hyper-v virtuális gépek](site-recovery-failback-from-azure-to-hyper-v.md#failback-to-an-alternate-location)

## <a name="service-providers"></a>Szolgáltatók
### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Szolgáltató vagyok. Működik a Site Recovery dedikált és megosztott infrastruktúra-modellekkel?
Igen, a Site Recovery támogatja mind a dedikált, mind a megosztott infrastruktúra-modelleket.

### <a name="for-a-service-provider-is-hello-identity-of-my-tenant-shared-with-hello-site-recovery-service"></a>A szolgáltató az hello identitásának hozzáfér a bérlők identitásadataihoz hello Site Recovery szolgáltatásban?
Nem. Bérlői azonosító névtelen marad. A bérlőnek nincs szüksége a hozzáférés toohello Site Recovery portálon. Csak az hello szolgáltatást nyújtó rendszergazdája kommunikál hello portálon.

### <a name="will-tenant-application-data-ever-go-tooazure"></a>Rendszer bérlő alkalmazásainak adatai eljutnak tooAzure?
Ha replikál szolgáltatás szolgáltató által birtokolt helyek között, az alkalmazásadatok soha nem kerülnek tooAzure. Az átvitel és a replikált hello szolgáltatást nyújtó helyei között közvetlenül az adatok titkosítása.

Ha tooAzure replikál, tooAzure tárolási, de nem a Site Recovery szolgáltatás toohello alkalmazás adatokat küldi el. Adatok titkosított az átvitel, és az Azure-ban is titkosítva maradnak.

### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Kapnak a bérlőim számlát valamilyen Azure-szolgáltatásról?
Nem. Azure számlázási kapcsolatban közvetlenül hello szolgáltató jelenti. A bérlők felé történő számlázásért a szolgáltató felel.

### <a name="if-im-replicating-tooazure-do-we-need-toorun-virtual-machines-in-azure-at-all-times"></a>Ha tooAzure replikációt végzek, szükség toorun virtuális gépek Azure-ban mindig?
Nem, az adatok Azure-tárfiók az előfizetésben replikált tooan. Ha feladatátvételi tesztet (vészhelyreállítási gyakorlatot) vagy tényleges feladatátvételt végez, a Site Recovery automatikusan létrehozza a virtuális gépeket az előfizetéséhez.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-tooazure"></a>Gondoskodik bérlő szintű elkülönítés amikor tooAzure-bA?
Igen.

### <a name="what-platforms-do-you-currently-support"></a>Jelenleg milyen platformok támogatottak?
Azure-csomag, a Cloud Platform System támogatott, és a System Center-alapú (2012 és újabb verzió) telepítések. [További](https://technet.microsoft.com/library/dn850370.aspx) Azure Pack csomaghoz és a Site Recovery integrálása.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Támogatott az egyetlen Azure Pack-re és az egyetlen VMM-kiszolgálóra alapuló üzembe helyezési modell?
Igen, replikálhatja a Hyper-V virtuális gépek tooAzure, és replikálhat szolgáltatói helyek között.  Vegye figyelembe, hogy ha a szolgáltatást nyújtó helyei között történik a replikáció, az Azure-forgatókönyvek integrációja nem érhető el.

## <a name="next-steps"></a>Következő lépések
* Olvasási hello [Site Recovery – áttekintés](site-recovery-overview.md)
* Információk a [Site Recovery architektúrájáról](site-recovery-components.md)  
