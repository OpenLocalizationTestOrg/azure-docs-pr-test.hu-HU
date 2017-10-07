---
title: "aaaAzure Site Recovery támogatási mátrixot az Azure tooAzure replikálásához |} Microsoft Docs"
description: "Hello támogatott operációs rendszerek és konfigurációk az Azure Site Recovery replikálása az Azure virtuális gépek (VM) összegzi az egy régióban tooanother a vész-helyreállítási igényekre."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/10/2017
ms.author: sujayt
ms.openlocfilehash: 75b2451b4c2069ca4b11deb0efe1329d43879eb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-site-recovery-support-matrix-for-replicating-from-azure-tooazure"></a>Az Azure tooAzure replikálásához az Azure Site Recovery támogatási mátrix


>[!NOTE]
>
> Az Azure virtuális gépek helyreállítási helyreplikálásának jelenleg előzetes verzió.

Ez a cikk az Azure Site Recovery replikálódik, és az Azure virtuális gépek helyreállítása egy régió tartozik tooanother régióban támogatott konfigurációk és összetevőket foglalja össze.

## <a name="user-interface-options"></a>A felhasználói felület lehetőségei

**Felhasználói felület** |  **Támogatott / nem támogatott**
--- | ---
**Azure Portal** | Támogatott
**Klasszikus portál** | Nem támogatott
**PowerShell** | Jelenleg nem támogatott.
**REST API** | Jelenleg nem támogatott.
**Parancssori felület** | Jelenleg nem támogatott.


## <a name="resource-move-support"></a>Erőforrás-áthelyezés támogatása

**Erőforrás-Áthelyezés típusa** | **Támogatott / nem támogatott** | **Megjegyzések**  
--- | --- | ---
**Erőforráscsoportok közötti áthelyezése közben tároló** | Nem támogatott |Erőforrás-csoportok közötti hello Recovery services-tároló nem helyezhető át.
**Számítási, tárolási és hálózati erőforráscsoportok közötti áthelyezése közben** | Nem támogatott |Ha egy virtuális gép (vagy a kapcsolódó összetevők, például a tárolási és hálózati) után a replikáció, toodisable replikációs kell, és újra hello virtuális gépek replikálásának engedélyezése.


## <a name="support-for-deployment-models"></a>Üzembe helyezési modellel támogatása

**Telepítési modell** | **Támogatott / nem támogatott** | **Megjegyzések**  
--- | --- | ---
**Klasszikus** | Támogatott | Csak a klasszikus virtuális gépek replikálása, és végezze el a helyreállítást a klasszikus virtuális gépként. Az erőforrás-kezelő virtuális gépként nem lehet helyreállítani. Ha a klasszikus virtuális gépek virtuális hálózati és közvetlenül az Azure-régió tooan nélkül telepíti, nem támogatott.
**Resource Manager** | Támogatott |

## <a name="support-for-replicated-machine-os-versions"></a>A replikált gép operációsrendszer-verziók támogatása

hello támogatási alatt is alkalmazható, az összes hello futó munkaterhelés említett operációs rendszer.

#### <a name="windows"></a>Windows

- Windows Server 2016 (Server Core és az asztali élmény Server) *
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2: legalább SP1

>[!NOTE]
>
> \*Windows Server 2016 Nano Server nem támogatott.

#### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 6.7, 6.8, 7.0, 7.1, 7.2, 7.3.
- CentOS 6.5, 6.6, 6.7, 6.8, 7.0, 7.1, 7.2, 7.3.
- Ubuntu 14.04 LTS Server [ (támogatott kernel verziók)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Ubuntu 16.04 LTS Server [ (támogatott kernel verziók)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Oracle Enterprise Linux 6.4 hello Red Hat kompatibilis kernel vagy szoros vállalati Kernel Release 3 (UEK3) futtató 6.5
- SUSE Linux Enterprise Server 11 SP3

>[!NOTE]
>
> Ubuntu kiszolgálók jelszóval hitelesítés és bejelentkezés, és a hello felhő inicializálás csomag tooconfigure felhő virtuális gépek használ, előfordulhat, hogy rendelkezik jelszó alapú esetén feladatátvevő (attól függően, hogy hello cloudinit konfigurációs.) letiltja a bejelentkezési Jelszóalapú bejelentkezési újból engedélyezhető hello virtuális gépen hello-beállítások menüjében (a támogatási hello + hibaelhárítás részt) a feladatátvételt a virtuális gépet az Azure-portálon hello hello hello jelszó alaphelyzetbe állításával.

### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Az Azure virtuális gépek támogatott Ubuntu kernel verziók

**Kiadás** | **Mobilitási szolgáltatás verziója** | **Kernel-verzió** |
--- | --- | --- |
14.04 LTS | 9.9 | 3.13.0-24-Generic too3.13.0 117-általános,<br/>too3.16.0-77-általános 3.16.0-25-Generic<br/>3.19.0-18-Generic too3.19.0 80 – Általános,<br/>4.2.0-18-Generic too4.2.0 42-általános,<br/>4.4.0-21-Generic too4.4.0 75 – általános |
14.04 LTS | 9.10 | 3.13.0-24-Generic too3.13.0 121-általános,<br/>too3.16.0-77-általános 3.16.0-25-Generic<br/>3.19.0-18-Generic too3.19.0 80 – Általános,<br/>4.2.0-18-Generic too4.2.0 42-általános,<br/>4.4.0-21-Generic too4.4.0 81-es – általános |
16.04 LTS | 9.10 | 4.4.0-21-Generic too4.4.0 81-általános,<br/>4.8.0-34-Generic too4.8.0 56-általános,<br/>4.10.0-14-Generic too4.10.0 24 – általános |

## <a name="supported-file-systems-and-guest-storage-configurations-on-azure-virtual-machines-running-linux-os"></a>Támogatott fájlrendszerek és a Vendég tárolási konfigurációk alakíthatók ki Linux operációs rendszert futtató Azure virtuális gépeken

* Fájlrendszer: ext3, ext4, ReiserFS (Suse Linux Enterprise Server csak), XFS
* Kötetkezelő: LVM2
* A többutas szoftver: eszköz leképezője

## <a name="region-support"></a>Terület támogatása

Replikáció és a virtuális gépek helyreállítása hello belül bármely két régiók közötti ugyanabban a földrajzi fürtben.

**Földrajzi fürt** | **Azure-régiók**
-- | --
Amerikai | Kanada keleti, Kanada központi, Dél-USA középső RÉGIÓJA, központi USA nyugati régiója, USA keleti régiója, USA keleti régiója 2. régiója, USA nyugati régiója, 2 USA nyugati régiója, USA középső RÉGIÓJA, Észak USA középső RÉGIÓJA
Európa | Egyesült Királyság nyugati régiója, Egyesült Királyság déli régiója, Észak-Európa, Nyugat-Európa
Ázsia | Dél-Indiában, közép-Indiában, Délkelet-Ázsiában, kelet-ázsiai keleti, japán, Nyugat-japán, koreai központi koreai Dél
Ausztrália   | Kelet-Ausztrália Ausztrália óceáni térség délkeleti régiója

>[!NOTE]
>
> Dél-Brazília régió csak replikálhatja, és biztonsági feladatátvételi tooone déli középső Régiójában, nyugati középső Régiójában, USA keleti régiója, USA keleti régiója 2. régiója, USA nyugati régiója, USA 2. nyugati és északi középső Régiójában régiók és sikertelen lesz.


## <a name="support-for-compute-configuration"></a>Számítási konfigurációhoz támogatása

**Konfigurálás** | **Támogatott/nem támogatott** | **Megjegyzések**
--- | --- | ---
Méret | Bármely Azure Virtuálisgép-méretet a CPU-magokat legalább 2 és 1 GB RAM | Tekintse meg a túl[Azure virtuálisgép-méretek](../virtual-machines/windows/sizes.md)
Rendelkezésre állási csoportok | Támogatott | Hello alapértelmezett beállítás "Replikáció engedélyezése" lépés során a portál használatakor hello rendelkezésre állási csoportok pedig automatikusan létrehozott adatforrását régió konfigurációja alapján. Módosíthatja a hello cél rendelkezésre állási csoport "replikált elemek > Beállítások > Számítás és hálózat > rendelkezésre állási csoport" bármikor.
Hibrid használata juttatás (HUB) virtuális gépek | Támogatott | Ha hello forrás virtuális gép HUB licenc engedélyezve van, hello feladatátvételi teszthez vagy feladatátvételi VM ugyancsak hello HUB licenc.
Virtuálisgép-méretezési csoportok | Nem támogatott |
A gyűjtemény lemezképei Azure - Microsoft közzétett | Támogatott | Támogatott, amíg a Site Recovery által támogatott operációs rendszeren futtatja hello VM
Azure-katalógus lemezképek - harmadik féltől származó közzétett | Támogatott | Mindaddig, amíg Site Recovery által támogatott operációs rendszeren futtatja hello VM támogatott.
Egyéni lemezképek - harmadik féltől származó közzétett | Támogatott | Mindaddig, amíg Site Recovery által támogatott operációs rendszeren futtatja hello VM támogatott.
Virtuális gépek áttelepítése a Site Recovery | Támogatott | Ha a rendszer a VMware vagy fizikai gépek áttelepítése a Site Recovery segítségével tooAzure, toouninstall hello régebbi verzióját a mobilitási szolgáltatás, és indítsa újra a hello gépek replikálása az Azure-régiót tooanother előtt.

## <a name="support-for-storage-configuration"></a>Tárolási konfiguráció támogatása

**Konfigurálás** | **Támogatott/nem támogatott** | **Megjegyzések**
--- | --- | ---
Maximális operációsrendszer-lemez mérete | 1023 GB | Tekintse meg a túl[virtuális gépek által használt lemezek.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Maximális adattároló lemezeinek mérete | 1023 GB | Tekintse meg a túl[virtuális gépek által használt lemezek.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Adatlemezek száma | Egy adott Azure virtuális gép mérete által támogatott legfeljebb 64 | Tekintse meg a túl[Azure virtuálisgép-méretek](../virtual-machines/windows/sizes.md)
Ideiglenes lemez | Mindig ki vannak zárva a replikációból | Ideiglenes lemez ki van zárva a replikációból mindig. Az Azure útmutatás szerint ideiglenes lemezen ne helyezzen állandó adatokat. Tekintse meg a túl[Azure virtuális gépeken mennyiségű ideiglenes lemezes](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk) további részleteket.
Adatok adatváltozási sebessége hello lemezen | Legfeljebb egy lemezen 6 MB/s | Ha hello átlagos átviteli sebességének módosítása a hello lemez túl 6 MB/s folyamatosan, replikációs fog nem dolgozza. Azonban ha egy alkalmanként adatokat kapacitásnövelés és hello adatmódosítási arány nagyobb, mint 6 MB/s egy kis ideig, és származik, replikációs fog szinkronizálásához. Ebben az esetben jelenhet meg némileg késleltetett helyreállítási pontokat.
Standard szintű storage-fiókok lemezek | Támogatott |
Prémium szintű storage-fiókok lemezek | Támogatott | Ha egy virtuális gép prémium és standard szintű storage-fiókok elosztva lemezzel rendelkezik, egy másik cél tárfiók minden lemez tooensure rendelkezik válassza hello azonos tárolási konfiguráció cél régióban
Standard szintű felügyelt lemez | Nem támogatott |  
Prémium szintű felügyelt lemez | Nem támogatott |
Tárolóhelyek | Támogatott |         
Titkosítását (SSE) | Támogatott | Gyorsítótár és a cél storage-fiókok válassza ki az engedélyezett SSE tárfiókot.     
Az Azure Disk Encryption (ADE) | Nem támogatott |
Gyakran használt adatok hozzáadása lemez | Nem támogatott | Hozzáadásakor, vagy távolítsa el a virtuális gép hello adatlemez, toodisable replikációs kell, és engedélyezze újra hello virtuális gép replikálását.
Lemez kizárása | Nem támogatott|   Ideiglenes lemez alapértelmezés szerint ki van zárva.
LRS | Támogatott |
GRS | Támogatott |
RA-GRS | Támogatott |
ZRS | Nem támogatott |  
Ritkán használt adatok és a gyakran használt adatok tárolási | Nem támogatott | Virtuálisgép-lemezek használata nem támogatott a ritkán használt adatok és a gyakran használt adatok tárolási

>[!IMPORTANT]
> Győződjön meg arról, hogy kövesse a hello [tárolási útmutatásokkal](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) a forrás Azure virtuális gépen tooavoid bármely teljesítményproblémákat. Ha követi hello alapértelmezett beállításokat, a Site Recovery hello forrás konfigurációtól függően szükséges hello tárfiókok hoz létre. Ha testre szabhatja, és válassza ki a saját beállításait, győződjön meg arról, hajtsa végre a hello (.. / storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks), a forrás virtuális gépeknek.

## <a name="support-for-network-configuration"></a>Hálózati konfiguráció támogatása
**Konfigurálás** | **Támogatott/nem támogatott** | **Megjegyzések**
--- | --- | ---
Hálózati adapter (NIC) | Legfeljebb egy adott Azure virtuális gép mérete által támogatott hálózati adapterek maximális száma | Hálózati adapter hello virtuális gép létrehozásakor a feladatátvételi teszt vagy a feladatátvételi művelet részeként jönnek létre. a virtuális gép hálózati adapterek hello forrás virtuális gép van a replikáció hello időpontjában hello száma függ hello feladatátvétel hálózati adapterek száma hello. Ha Ön hozzáadása a hálózati adapter replikáció engedélyezése után, nem befolyásolja hello feladatátvevő VM hálózati száma.
Internetes Load Balancer | Támogatott | Tooassociate kell hello előre konfigurálva van egy azure Automation szolgáltatásbeli parancsfájl használata a helyreállítási tervben terheléselosztóhoz.
Belső terheléselosztó | Támogatott | Tooassociate kell hello előre konfigurálva van egy azure Automation szolgáltatásbeli parancsfájl használata a helyreállítási tervben terheléselosztóhoz.
Nyilvános IP-cím| Támogatott | Egy már meglévő nyilvános IP toohello NIC tooassociate kell vagy létrehoz egy, toohello hálózati adapter egy azure Automation szolgáltatásbeli parancsfájl használata a helyreállítási tervet.
NSG a hálózati Adapteren (erőforrás-kezelő)| Támogatott | Tooassociate hello NSG toohello egy azure Automation szolgáltatásbeli parancsfájl használata a helyreállítási tervben szereplő hálózati adapter van szüksége.  
NSG alhálózat (Resource Manager és klasszikus)| Támogatott | Tooassociate hello NSG toohello egy azure Automation szolgáltatásbeli parancsfájl használata a helyreállítási tervben szereplő hálózati adapter van szüksége.
NSG a virtuális gép (klasszikus)| Támogatott | Tooassociate hello NSG toohello egy azure Automation szolgáltatásbeli parancsfájl használata a helyreállítási tervben szereplő hálózati adapter van szüksége.
Fenntartott IP (statikus IP-cím) / megőrizni a forrás IP-címe | Támogatott | Ha hello hello forrás virtuális gép hálózati adapter statikus IP-konfigurációt és a cél alhálózathoz hello rendelkezik hello azonos IP érhető el, toohello feladatátvételi VM van hozzárendelve. Ha hello cél alhálózat nem rendelkezik azonos IP érhető el, egy elérhető IP-címek a hello hello hello alhálózatot a virtuális gép van fenntartva. Megadhat egy fix IP-cím az Ön által választott, a "replikált elemek > Beállítások > Számítás és hálózat > hálózati illesztőt. Válassza ki a hálózati adapter hello, és hello alhálózatot és IP-címe a kiválasztott adjon meg.
Dinamikus IP| Támogatott | Ha hello hello forrás virtuális gép hálózati kártya dinamikus IP-konfiguráció, hello hálózati adapterén hello feladatátvételi virtuális gép is alapértelmezés szerint dinamikus. Megadhat egy fix IP-cím az Ön által választott, a "replikált elemek > Beállítások > Számítás és hálózat > hálózati illesztőt. Válassza ki a hálózati adapter hello, és hello alhálózatot és IP-címe a kiválasztott adjon meg.
A Traffic Manager integrálása | Támogatott | Előre beállíthatja a a traffic manager úgy, hogy a hello forgalom irányított toohello végpont rendszeres időközönként és toohello végpont cél régióban feladatátvétel esetén a forrás-régióban.
Azure DNS által felügyelt | Támogatott |
Egyéni DNS  | Támogatott |    
Nem hitelesített Proxy | Támogatott | Tekintse meg a túl[hálózati útmutató.](site-recovery-azure-to-azure-networking-guidance.md)    
Hitelesített Proxy | Nem támogatott | Ha hello VM egy hitelesített proxykiszolgálót használ a kimenő hálózati kapcsolatot, akkor nem replikálhatók Azure Site Recovery segítségével.  
Hely tooSite VPN helyszíni (vagy anélkül ExpressRoute)| Támogatott | Győződjön meg arról, hogy úgy, hogy hello hely helyreállítási forgalom nem útválasztásos tooon helyszíni hello udr-EK és NSG-k vannak konfigurálva. Tekintse meg a túl[hálózati útmutató.](site-recovery-azure-to-azure-networking-guidance.md)  
Virtuális hálózat tooVNET kapcsolat | Támogatott | Tekintse meg a túl[hálózati útmutató.](site-recovery-azure-to-azure-networking-guidance.md)  


## <a name="next-steps"></a>Következő lépések
- További információ [hálózati útmutatót az Azure virtuális gépek replikálása](site-recovery-azure-to-azure-networking-guidance.md)
- A munkaterhelések számára a védelmének megkezdéséhez [Azure virtuális gépek replikálása](site-recovery-azure-to-azure.md)
