---
title: "az Azure Site Recovery segítségével replikációs tooAzure aaaPrerequisites |} Microsoft Docs"
description: "További információk a hello előfeltételei replikáló virtuális gépek és fizikai gépek tooAzure hello Azure Site Recovery szolgáltatással."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/23/2017
ms.author: rajanaki
ms.openlocfilehash: 0e32ab7cd7c65a3f67ffa2f2c15af189c15b6f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#  <a name="prerequisites-for-replication-from-on-premises-tooazure-by-using-site-recovery"></a>A Site Recovery segítségével helyszíni tooAzure replikációs előfeltételei

> [!div class="op_single_selector"]
> * [Az Azure tooAzure replikáláshoz](site-recovery-azure-to-azure-prereq.md)
> * [A helyszíni tooAzure replikáláshoz](site-recovery-prereq.md)

Az Azure Site Recovery segítségével az üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégia támogatja történő replikálásával egy Azure virtuális gép (VM) tooanother Azure-régiót. A Site Recovery is a helyszíni fizikai kiszolgálók és virtuális gépek toohello Azure-felhőbe vagy tooa másodlagos adatközpontba replikálja. Nem tervezett kimaradás esetén az elsődleges helyen, akkor átveheti tooa másodlagos helyre tookeep alkalmazások és számítási feladatok nem állnak. Hátsó tooyour elsődleges helye sikertelen lehet, ha az elsődleges hely hello toonormal műveletek adja vissza. További információ a Site Recovery: [Mi az Site Recovery?](site-recovery-overview.md).

Ez a cikk azt a Site Recovery replikálást az a helyi gép tooAzure a kezdő hello Előfeltételek foglalják össze.

Bármely fűzhetnek hello cikk hello alján. Is megkérheti a hello technikai kérdésekhez [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="azure-requirements"></a>Azure-követelmények

**Követelmény** | **Részletek**
--- | ---
**Azure-fiók** | A [Microsoft Azure-fiók](http://azure.microsoft.com/). Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.
**Site Recovery szolgáltatás** | Az Azure Site Recovery szolgáltatás hello díjszabása kapcsolatos további információkért lásd: [Site Recovery díjszabásáról](https://azure.microsoft.com/pricing/details/site-recovery/). |
**Azure Storage** | Szüksége van egy Azure Storage-fiók toostore replikált adatokat. hello tárfiókot kell hello megegyező régióban hello Azure Recovery Services-tároló. Azure virtuális gépek jönnek létre, ha feladatátvétel történik.<br/><br/> Attól függően, hogy az erőforrás-modellje hello Azure virtuális gép feladatátvételi kívánt toouse, beállíthat egy fiók hello segítségével [Azure Resource Manager üzembe helyezési modellben](../storage/common/storage-create-storage-account.md) vagy hello [klasszikus üzembe helyezési modellel](../storage/common/storage-create-storage-account.md).<br/><br/>[Georedundáns tárolást](../storage/common/storage-redundancy.md#geo-redundant-storage) és helyileg redundáns tárolást is használhat. A georedundáns tárolást javasoljuk. Georedundáns tárolás az adatokat is lehetséges regionális kimaradás során, vagy ha az elsődleges régióban hello nem állíthatók vissza.<br/><br/> Használhatja a szabványos Azure storage-fiók, vagy a prémium szintű Azure Storage is használhat. [Prémium szintű storage](https://docs.microsoft.com/azure/storage/storage-premium-storage) jellemzően akkor alkalmazzák egy i/o-teljesítmény egységesen magas és alacsony késést igénylő virtuális gépekhez. Prémium szintű Storage a virtuális gépek üzemeltethet I/O-igényes munkaterhelések. Ha Prémium szintű tárolást használ a replikált adatokhoz, Standard szintű tárfiókra is szüksége van. Standard szintű tárfiókot a replikálási naplókhoz, folyamatban lévő változtatások tooon helyszíni adatok rögzítéséhez tárolja.<br/><br/>
**Tárolási korlátai** | Nem helyezhető át a storage-fiókok, amelyekkel a Site Recovery tooa másik erőforráscsoportban található, és nem áthelyezése egy másik előfizetés tooor használatára.<br/><br/> Jelenleg toopremium tárfiókok közép-Indiában és Dél-Indiában replikálása nem érhető el.
**Azure-hálózat** | Egy Azure-hálózatra van szüksége, toowhich Azure virtuális gépek a feladatátvételt követően csatlakozhatnak. hello Azure hálózat kell hello hello és ugyanabban a régióban Recovery Services-tároló.<br/><br/> Hello Azure-portálon, létrehozhat egy Azure-hálózatra hello segítségével [Resource Manager üzembe helyezési modellben](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) vagy hello [klasszikus üzembe helyezési modellel](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).<br/><br/> Ha a System Center Virtual Machine Manager (VMM) tooAzure replikálja, be kell állítania a hálózatra való leképezést a VMM Virtuálisgép-hálózatok és az Azure-hálózatok között. Ez biztosítja, hogy Azure virtuális gépek a feladatátvételt követően tooappropriate hálózatok csatlakoztatása.
**Hálózati korlátozásai** | Nem helyezhető át a Site Recovery tooa másik erőforráscsoportban található hálózati fiókkal, és nem áthelyezése egy másik előfizetés tooor használatára.
**Hálózatleképezés** | Ha a Microsoft Hyper-V virtuális gépek VMM-felhőkben replikálja, be kell állítania a hálózat leképezését. Ez biztosítja, hogy Azure virtuális gépek tooappropriate hálózatok csatlakoztatása, ha a feladatátvételt követően jönnek létre.

> [!NOTE]
> hello következő részek a hello felhasználói környezet különböző összetevői hello előfeltételeit. Specifikus konfigurációk-támogatással kapcsolatos további információkért lásd: hello [támogatási mátrix](site-recovery-support-matrix.md).
>

## <a name="disaster-recovery-of-vmware-vms-or-physical-windows-or-linux-servers-tooazure"></a>VMware virtuális gépek vagy fizikai Windows vagy Linux rendszerű kiszolgálók tooAzure katasztrófa utáni helyreállítás
hello a következő összetevők szükségesek a VMware virtuális gépek vagy windowsos vagy Linuxos fizikai kiszolgálók vész-helyreállítási. Ezek a ezenkívül azokat, leírt toohello [Azure-követelményeknek](#azure-requirements).


### <a name="configuration-server-or-additional-process-server"></a>Konfigurációs kiszolgáló, vagy további folyamatkiszolgáló

Hello helyszíni hely és az Azure közötti hello konfigurációs kiszolgáló tooorchestrate kommunikáció egy helyszíni gépre állíthatja be. hello a helyi számítógépen is adatreplikáció kezeli. <br/></br>

*   **VMware vCenter vagy vSphere-gazdagép Előfeltételek**

    | **Összetevő** | **Követelmények** |
    | --- | --- |
    | **vSphere** | Egy vagy több VMware vSphere hipervizorok kell rendelkeznie.<br/><br/>Hipervizorok verziót kell futtatnia vSphere 6.0, 5.5 vagy 5.1, hello a legújabb frissítéseket.<br/><br/>Azt javasoljuk, hogy vSphere gazdagépek és vCenter-kiszolgáló lehet a hello azonos hálózati hello folyamat kiszolgálóként. Ha beállította egy dedikált folyamatkiszolgáló, ez a hello hálózati ahol hello konfigurációs kiszolgáló. |
    | **vCenter** | Azt javasoljuk, hogy telepít egy VMware vCenter server toomanage a vSphere gazdagépek. Az verziót kell futtatnia vCenter 6.0 vagy 5.5, hello legújabb frissítéseit.<br/><br/>**A korlátozás**: a Site Recovery nem támogatja a replikációt vCenter vMotion példányai között. Tárolás DRS és a Storage vmotion szolgáltatások használata is nem támogatottak a fő célkiszolgáló virtuális gép védelem-újrabeállítási művelet után.||

* **A replikált gép előfeltételei**

    | **Összetevő** | **Követelmények** |
    | --- | --- |
    | **A helyszíni gépeket** (a VMware virtuális gépek) | A replikált virtuális gépek telepítve és fut a VMware-eszközök kell rendelkeznie.<br/><br/> Meg kell felelnie a virtuális gépek [Azure-Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) egy Azure virtuális gép létrehozásához.<br/><br/>Minden védett számítógépen a lemezkapacitás 1,023 GB-nál több nem lehet. <br/><br/>Egy minimális 2 GB szabad terület a hello telepítési meghajtón összetevő telepítése szükséges.<br/><br/>Ha azt szeretné, hogy a virtuális Gépre kiterjedő konzisztencia tooenable, 20004 portot kell megnyitni a hello virtuális gép helyi tűzfalon.<br/><br/>Számítógép nevének 1 és 63 karakter között (használható betűket, számokat és kötőjeleket) kell lennie. hello nevét kell kezdődnie, betűvel vagy számmal, és betűvel vagy számmal végződhet. <br/><br/>A gépek replikációjának engedélyezése után hello Azure nevét módosíthatja.<br/><br/> |
    | **Windows-alapú gépek** (fizikai vagy VMware) | hello gépnek futnia kell hello következő támogatott 64 bites operációs rendszerek: <br/>-Windows Server 2012 R2 rendszerben<br/>-Windows Server 2012-ben<br/>-Windows Server 2008 R2 SP1 vagy újabb verzió<br/><br/> hello operációs rendszert kell lennie a c meghajtó hello OS lemezen telepítve kell lennie Windows alaplemez, és nem dinamikus. hello adatlemez dinamikus lehet.<br/><br/>|
    | **Linux-gépek** (fizikai vagy VMware) | hello gépnek futnia kell hello következő támogatott 64 bites operációs rendszerek: <br/>-Red Hat Enterprise Linux 7.2, 7.1, 6.8 vagy 6.7<br/>-Centos 7.2, 7.1, 7.0, 6.8, 6.7, 6.6 vagy 6.5<br/>-Kiszolgáló Ubuntu 14.04 LTS (Ubuntu támogatott kernel-verziók listáját lásd: [támogatott operációs rendszerek](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions))<br/>-Oracle Enterprise Linux 6.5-ös vagy 6.4, futó vagy hello Red Hat-kompatibilis kernel vagy szoros vállalati Kernel Release 3 (UEK3)<br/>-SUSE Linux Enterprise Server 11 SP4 vagy SUSE Linux Enterprise Server 11 SP3<br/><br/>A/Etc/Hosts fájlt a védett gépek bejegyzéseit, amelyek hozzárendeléséről hello helyi állomás neve tooIP minden hálózati adapterekkel társított kell rendelkeznie.<br/><br/>A feladatátvétel után ha tooconnect tooan egy Secure Shell (SSH) ügyfél Linux rendszert futtató Azure virtuális Gépen, győződjön meg arról, hogy hello SSH szolgáltatást a védett hello számítógépen értéke toostart automatikusan a rendszer indításakor. Győződjön meg arról is, hogy a tűzfalszabályok engedélyezik-e egy SSH-kapcsolat toohello védett gép.<br/><br/>hello állomásnév, csatlakoztatási pontokat, eszköz, és Linux rendszer elérési utakat és fájlneveket (például a /etc/ és a/usr) kell angolul csak.<br/><br/>következő könyvtárak hello (Ha külön partíciókként beállítása vagy fájlrendszerek) kell lennie a hello azonos (lemez az operációs rendszer hello) hello forráskiszolgálón:<br/>- / (root)<br/>- / rendszerindító<br/>-/ usr<br/>-/ usr/helyi<br/>-/var<br/>- / stb<br/><br/>Jelenleg XFS v5 funkciók – például a metaadatok ellenőrzőösszeg nem támogatottak a Site Recovery által XFS fájlrendszeren. Győződjön meg arról, hogy a XFS fájlrendszerek nem használják az v5 szolgáltatásokat. Hello xfs_info segédprogram toocheck hello XFS superblock hello partíció esetében is használhatja. Ha **ftype** értéke túl**1**, XFS v5 szolgáltatásokat használják.<br/><br/>Red Hat Enterprise Linux 7 és a CentOS 7 kiszolgálókon hello lsof segédprogram telepítve és elérhetőnek kell lenniük.<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-tooazure-no-vmm"></a>A Hyper-V virtuális gépek tooAzure (nincs VMM) katasztrófa utáni helyreállítás

hello a következő összetevők szükségesek a vész-helyreállítási Hyper-V virtuális gépek VMM-felhőkben. Ezek a ezenkívül azokat, leírt toohello [Azure-követelményeknek](#azure-requirements).

| **Előfeltétel** | **Részletek** |
| --- | --- |
| **Hyper-V gazdagép** |Egy vagy több helyszíni kiszolgálók futnia kell a Windows Server 2012 R2 hello legújabb frissítésekkel és hello Hyper-V szerepkör engedélyezve van vagy Microsoft Hyper-V Server 2012 R2.<br/><br/>Hyper-V kiszolgálók rendelkeznie kell egy vagy több virtuális gépeket.<br/><br/>Hyper-V-kiszolgálóknak kell lenniük a csatlakoztatott toohello Internet, közvetlen vagy proxyn keresztülmenő.<br/><br/>Hyper-V-kiszolgálóknak rendelkezniük kell a Tudásbázis következő cikke hello ismertetett hello javítások [2961977](https://support.microsoft.com/kb/2961977) telepítve.
|**Provider és Agent**| Site Recovery üzembe helyezése során hello Azure Site Recovery provider telepítése. hello provider telepítése is telepíti a hello Azure Recovery Services Agent ügynököt futtató virtuális gépek, amelyek minden Hyper-V kiszolgálón tooprotect szeretné. <br/><br/>A Site Recovery tárolóban kell lennie minden Hyper-V kiszolgálók hello hello provider és hello agent azonos verzióit.<br/><br/>hello ellátót tooconnect tooSite helyreállítási hello interneten keresztül. Közvetlen vagy proxyn keresztüli forgalmat lehet küldeni. HTTPS-alapú proxyk nem támogatott. hello proxykiszolgáló engedélyeznie kell a következő URL-címek hozzáférési toohello:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/>Ha IP-címeken alapuló tűzfalszabályok szabályok hello kiszolgálón, győződjön meg arról, hogy hello szabályok engedélyezik a kommunikációt tooAzure.<br/><br/> Hello engedélyezése [Azure datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443-as port).<br/><br/> Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint a hello nyugati USA (hozzáférés-vezérlés és identitás kezelésre szolgáló).


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-tooazure"></a>A Hyper-V virtuális gépek a VMM vész-helyreállítási felhők tooAzure

hello a következő összetevők szükségesek a vész-helyreállítási Hyper-V virtuális gépek VMM-felhőkben. Ezek a ezenkívül azokat, leírt toohello [Azure-követelményeknek](#azure-requirements).

| **Előfeltétel** | **Részletek** |
| --- | --- |
| **A Virtual Machine Manager** |Rendelkeznie kell legalább egy VMM-kiszolgálókon futó System Center 2012 R2 vagy újabb verzió. Minden VMM-kiszolgálót kell rendelkeznie a konfigurált egy vagy több felhőt. <br/><br/>Felhő kell rendelkeznie:<br/>– Egy vagy több VMM-gazdagépcsoportot.<br/>– Egy vagy több Hyper-V gazdakiszolgálók vagy fürtök minden gazdagépcsoportban.<br/><br/>VMM-felhőkben beállításával kapcsolatos további információkért lásd: [hogyan toocreate a Virtual Machine Manager 2012 felhő](http://social.technet.microsoft.com/wiki/contents/articles/2729.how-to-create-a-cloud-in-vmm-2012.aspx). |
| **Hyper-V** |Hyper-V gazdakiszolgálókon legalább futnia kell a Windows Server 2012 R2 hello Hyper-V szerepkör engedélyezve van, vagy a Microsoft Hyper-V Server 2012 R2. hello legújabb frissítéseket kell telepíteni.<br/><br/> A Hyper-V server rendelkeznie kell egy vagy több virtuális gépeket.<br/><br/> A Hyper-V gazdakiszolgáló vagy fürt, amelyet az tooreplicate virtuális gépeket a VMM-felhőben kell kezelni.<br/><br/>Hyper-V-kiszolgálóknak kell lenniük a csatlakoztatott toohello Internet, közvetlen vagy proxyn keresztülmenő.<br/><br/>Hyper-V-kiszolgálóknak rendelkezniük kell a Tudásbázis következő cikke hello ismertetett hello javítások [2961977](https://support.microsoft.com/kb/2961977) telepítve.<br/><br/>Hyper-V gazdakiszolgálók adatok replikációs tooAzure Internet-hozzáférés szükséges. |
| **Provider és Agent** |Során az Azure Site Recovery üzembe helyezése Telepítse az Azure Site Recovery Provider hello VMM-kiszolgálón. Recovery Services Agent telepítése a Hyper-V gazdagépekre. hello provider és agent kell tooconnect tooAzure hello interneten keresztül közvetlenül, vagy olyan proxyn keresztül. A HTTPS-alapú proxyk nem támogatottak. hello proxykiszolgáló hello VMM-kiszolgáló és a Hyper-V-gazdagépek hozzáférést kell engedélyeznie: <br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Ha IP-címeken alapuló tűzfalszabályok szabályok hello VMM-kiszolgálón, győződjön meg arról, hogy hello szabályok engedélyezik a kommunikációt tooAzure.<br/><br/> Hello engedélyezése [Azure datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653) és a HTTPS (443-as port).<br/><br/>IP-címtartományát hello Azure-régió, az előfizetés, és az hello nyugati USA (hozzáférés-vezérlés és identitás kezelésre szolgáló) engedélyezése.<br/><br/> |

### <a name="replicated-machine-prerequisites"></a>A replikált gép előfeltételei

| **Összetevő** | **Részletek** |
| --- | --- |
| **Védett virtuális gépek** | A Site Recovery támogatja az összes operációs rendszer által támogatott [Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).<br/><br/>Virtuális gépek meg kell felelnie a hello [Azure-Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) egy Azure virtuális gép létrehozásához. Számítógép nevének 1 és 63 karakter között (használható betűket, számokat és kötőjeleket) kell lennie. hello nevét kell kezdődnie, betűvel vagy számmal, és betűvel vagy számmal végződhet. <br/><br/>Hello nevet a virtuális gép hello virtuális gép replikációjának bekapcsolását követően módosíthatja. <br/><br/> Minden védett gép lemezkapacitás 1,023 GB-nál több nem lehet. A virtuális gépek too16 lemezeket (felfelé too16 TB) lehet.<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-tooa-customer-owned-site"></a>A Hyper-V virtuális gépek a VMM-felhők tooa ügyfél által birtokolt helyek katasztrófa utáni helyreállítás

a Hyper-V virtuális gépek a VMM-felhők tooa ügyfél által birtokolt helyek vész-helyreállítási hello a következő összetevők szükségesek. Ezek a ezenkívül azokat, leírt toohello [Azure-követelményeknek](#azure-requirements).

| **Összetevő** | **Részletek** |
| --- | --- |
| **A Virtual Machine Manager** |  Azt javasoljuk, hogy a hello elsődleges hely és a másodlagos hely hello VMM-kiszolgáló központi telepítése.<br/><br/> Is [replikálása egy VMM-kiszolgálón felhők közötti](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment). egyetlen VMM-kiszolgálón felhők közötti tooreplicate, hello VMM-kiszolgálón konfigurált legalább két felhők kell.<br/><br/> VMM-kiszolgálókon legalább futnia kell a System Center 2012 SP1, hello legújabb frissítéseit.<br/><br/> Minden VMM-kiszolgálót rendelkeznie kell egy vagy több felhőt. Az összes felhő Hyper-V kapacitás-készlet hello kell rendelkeznie. <br/><br/>Felhők rendelkeznie kell legalább egy VMM-gazdagépcsoportot. VMM-felhőkben beállításával kapcsolatos további információkért lásd: [Azure Site Recovery üzembe helyezésének előkészítése](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric). |
| **Hyper-V** | Hyper-V kiszolgálók legalább futnia kell hello Hyper-V szerepkör és a Windows Server 2012 engedélyezve van, és rendelkezik hello telepített legújabb frissítések.<br/><br/> A Hyper-V server rendelkeznie kell egy vagy több virtuális gépeket.<br/><br/>  Hyper-V gazdakiszolgálók gazdagépcsoportokban hello elsődleges és másodlagos VMM-felhőkben kell elhelyezkedniük.<br/><br/> Ha a Hyper-V fürt, a Windows Server 2012 R2 rendszeren futtatja, azt javasoljuk, hogy a Tudásbázis következő cikke hello frissítés telepítése [2961977](https://support.microsoft.com/kb/2961977).<br/><br/> Ha a Hyper-V fürt futó Windows Server 2012 és a statikus IP cím alapú fürtöt, nem a fürtszervező automatikusan jön létre. Hello fürtszervező manuálisan kell konfigurálni. Hello fürtszervező kapcsolatos további információkért lásd: [konfigurálása hello replika fürtszervező szerepkört a fürtök közötti replikáció](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Szolgáltató** | Site Recovery üzembe helyezése során telepítse hello Azure Site Recovery provider VMM-kiszolgálókon. hello szolgáltató tooorchestrate replikációs HTTPS (443-as porton) keresztül kommunikál a Site Recovery. Adatok replikáció hello elsődleges és másodlagos Hyper-V kiszolgálók között hello LAN keresztül vagy a VPN-kapcsolaton keresztül.<br/><br/> hello VMM-kiszolgálón futó szolgáltató hello kell hozzáférni a következő URL-címek toohello:<br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>hello Site Recovery provider engedélyeznie kell a VMM-kiszolgálók toohello hello tűzfal kommunikáció [Azure datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és teszi lehetővé a hello HTTPS (443-as port) protokollt. |


## <a name="url-access"></a>URL hozzáférés
Az URL-címek VMware, a VMM és a Hyper-V gazdakiszolgálók elérhetőnek kell lennie:

|**URL-CÍME** | **A VMM tooVMM** | **A VMM tooAzure** | **Hyper-V tooAzure** | **VMware tooAzure** |
|--- | --- | --- | --- | --- |
|``*.accesscontrol.windows.net`` | Engedélyezés | Engedélyezés | Engedélyezés | Engedélyezés |
|``*.backup.windowsazure.com`` | Nem szükséges | Engedélyezés | Engedélyezés | Engedélyezés |
|``*.hypervrecoverymanager.windowsazure.com`` | Engedélyezés | Engedélyezés | Engedélyezés | Engedélyezés |
|``*.store.core.windows.net`` | Engedélyezés | Engedélyezés | Engedélyezés | Engedélyezés |
|``*.blob.core.windows.net`` | Nem szükséges | Engedélyezés | Engedélyezés | Engedélyezés |
|``https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi`` | Nem szükséges | Nem szükséges | Nem szükséges | SQL letölthető engedélyezése |
|``time.windows.com`` | Engedélyezés | Engedélyezés | Engedélyezés | Engedélyezés|
|``time.nist.gov`` | Engedélyezés | Engedélyezés | Engedélyezés | Engedélyezés |
