---
title: "aaaAzure Site Recovery támogatási mátrix tooAzure replikálásához |} Microsoft Docs"
description: "Az Azure Site Recovery összegzi a hello támogatott operációs rendszerek és összetevők"
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: rajanaki
ms.openlocfilehash: eae1db2ff1392d272f6b2eb0e3410da19d09da7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-site-recovery-support-matrix-for-replicating-from-on-premises-tooazure"></a>A helyszíni tooAzure replikálásához az Azure Site Recovery támogatási mátrix


Ez a cikk az Azure Site Recovery replikálásához és helyreállítása tooAzure támogatott konfigurációk és összetevőket foglalja össze. Az Azure Site Recovery követelményeiről kapcsolatban bővebben lásd: hello [Előfeltételek](site-recovery-prereq.md).


## <a name="support-for-deployment-options"></a>Telepítési lehetőségek támogatása

**Üzembe helyezés** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a/nélkül a Virtual Machine Manager)** |
--- | --- | ---
**Azure Portal** | Helyszíni VMware virtuális gépek tooAzure tárolás, az Azure Resource Manager vagy a hagyományos tárolási és a hálózatok.<br/><br/> Feladatátvételi tooResource Manager vagy a klasszikus virtuális gépeket. | A helyszíni Hyper-V virtuális gépek tooAzure tároló, a Resource Manager és a hagyományos tárolási és a hálózatok.<br/><br/> Feladatátvételi tooResource Manager vagy a klasszikus virtuális gépeket.
**Klasszikus portál** | Karbantartási mód csak. Nem hozható létre új tárolók. | Karbantartási mód csak.
**PowerShell** | Jelenleg nem támogatott. | Támogatott


## <a name="support-for-datacenter-management-servers"></a>Adatközpont-felügyeleti kiszolgálók támogatása

### <a name="virtualization-management-entities"></a>Virtualizálási felügyeleti entitások

**Üzembe helyezés** | **Támogatás**
--- | ---
**VMware virtuális gép vagy fizikai kiszolgáló** | vCenter 6.5, 6.0 vagy 5.5
**A Hyper-V (Virtual Machine Managerrel)** | A System Center Virtual Machine Manager 2016 és a System Center Virtual Machine Manager 2012 R2

  >[!Note]
  > Windows Server 2016 és 2012 R2-állomások a System Center Virtual Machine Manager 2016 felhő jelenleg nem támogatott.

### <a name="host-servers"></a>Kiszolgálók

**Üzembe helyezés** | **Támogatás**
--- | ---
**VMware virtuális gép vagy fizikai kiszolgáló** | vSphere 6.5, 6.0, 5.5
**A Hyper-V (a/nélkül a Virtual Machine Manager)** | Windows Server 2016-ot, a Windows Server 2012 R2 legújabb frissítéseit.<br></br>Az SCVMM használata esetén a Windows Server 2016 állomások SCVMM 2016 kell kezelnie.


  >[!Note]
  >A Hyper-V helyet, amely a Windows Server 2016 és 2012 R2 rendszert futtató gazdagépeken keveri jelenleg nem támogatott. Helyreállítási tooan másik helyre a virtuális gépek olyan Windows Server 2016 gazdagépen jelenleg nem támogatott.

## <a name="support-for-replicated-machine-os-versions"></a>A replikált gép operációsrendszer-verziók támogatása

Meg kell felelnie a védett virtuális gépek [Azure-követelményeknek](#failed-over-azure-vm-requirements) tooAzure replikálása esetén.
a következő táblázat hello replikált operációs rendszer támogatásának különböző telepítési forgatókönyvek az Azure Site Recovery használata során foglalja össze. Ez a támogatás nem alkalmazható, az összes hello futó munkaterhelés említett operációs rendszer.

 **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a/VMM nélkül)** |
--- | --- |
64 bites Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2: legalább SP1<br/>*Windows Server 2016* – jelenleg a VMware virtuális gépek és fizikai kiszolgálókon nem támogatott. <br/><br/> Red Hat Enterprise Linux: 5.2-es too5.11, 6.1 too6.8, 7.0 too7.3 <br/><br/>Centos: 5.2-es too5.11, 6.1 too6.8, 7.0 too7.3 <br/><br/>Ubuntu 14.04 LTS server[ (támogatott kernel verziók)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Ubuntu 16.04 LTS server[ (támogatott kernel verziók)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Oracle Enterprise Linux 6.4 hello Red Hat kompatibilis kernel vagy szoros vállalati Kernel Release 3 (UEK3) futtató 6.5 <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> SUSE Linux Enterprise Server 11 SP4 <br/>(A gépek replikálásához SLES 11 SP3 tooSLES 11 SP4 frissítés nem támogatott. Ha a replikált gép SLES 11SP3 tooSLES 11 SP4 frissítették, meg lesz toodisable replikációs kell és újra post hello frissítés hello gép védelméhez.) | A vendég operációs rendszer [Azure által támogatott](https://technet.microsoft.com/library/cc794868.aspx)


>[!IMPORTANT]
>(Alkalmazható tooVMware/fizikai kiszolgálók replikálása tooAzure)
>
> Red Hat Enterprise Linux Server 7 + és CentOS 7 + kiszolgálókon, kernel verzió 3.10.0-514 verziójától kezdve alkalmazható hello Azure Site Recovery mobilitási szolgáltatás 9.8 verziójának.<br/><br/>
> Ügyfelek hello 3.10.0-514 kernel verziójával készült hello mobilitásiszolgáltatás-verziót 9.8 kisebb a szükséges toodisable replikáció, a hello mobilitási szolgáltatás tooversion 9.8 frissítés hello verzióját, majd újra a replikáció engedélyezése.


### <a name="supported-ubuntu-kernel-versions-for-vmwarephysical-servers"></a>Ubuntu kernel támogatott verziók, VMware vagy fizikai kiszolgálók

**Kiadás** | **Mobilitási szolgáltatás verziója** | **Kernel-verzió** |
--- | --- | --- |
14.04 LTS | 9.9 | 3.13.0-24-Generic too3.13.0 117-általános,<br/>too3.16.0-77-általános 3.16.0-25-Generic<br/>3.19.0-18-Generic too3.19.0 80 – Általános,<br/>4.2.0-18-Generic too4.2.0 42-általános,<br/>4.4.0-21-Generic too4.4.0 75 – általános |
14.04 LTS | 9.10 | 3.13.0-24-Generic too3.13.0 121-általános,<br/>too3.16.0-77-általános 3.16.0-25-Generic<br/>3.19.0-18-Generic too3.19.0 80 – Általános,<br/>4.2.0-18-Generic too4.2.0 42-általános,<br/>4.4.0-21-Generic too4.4.0 81-es – általános |
16.04 LTS | 9.10 | 4.4.0-21-Generic too4.4.0 81-általános,<br/>4.8.0-34-Generic too4.8.0 56-általános,<br/>4.10.0-14-Generic too4.10.0 24 – általános |


## <a name="supported-file-systems-and-guest-storage-configurations-on-linux-vmwarephysical-servers"></a>Támogatott fájlrendszerek és a Vendég tárolási konfigurációk alakíthatók ki Linux (VMware vagy fizikai kiszolgálók)

hello következő fájl, rendszerek és a tárolási konfiguráció szoftvereket támogatja a VMware vagy fizikai kiszolgálókon futó Linux-kiszolgálókon:
* Fájlrendszer: ext3, ext4, ReiserFS (Suse Linux Enterprise Server csak), XFS
* Kötetkezelő: LVM2
* A többutas szoftver: eszköz leképezője

Paravirtualized tárolási eszközök (az exportált paravirtualized-illesztőprogramok) nem támogatottak.<br/>
Több sor blokk IO eszközökön nem támogatottak.<br/>
Fizikai kiszolgálók hello HP CCISS tárolóvezérlő nem támogatottak.<br/>

>[!Note]
> A következő könyvtárak Linux kiszolgálók hello (Ha külön partíciók /-fájlrendszerek beállított) kell lennie a hello azonos (lemez az operációs rendszer hello) hello forráskiszolgálón: / (gyökér), / Boot, / usr, /usr/local, /var, etc<br/><br/>
> Például a metaadatok ellenőrzőösszeg XFS fájlrendszerek XFSv5 funkcióinak támogatottak, kezdve a mobilitási szolgáltatás hello 9.10 verzióját. Ha XFSv5 funkciókat használ, győződjön meg arról, 9.10 vagy későbbi Mobilitásiszolgáltatás-verziót futtat. Hello xfs_info segédprogram toocheck hello XFS superblock hello partíció esetében is használhatja. Ha ftype too1, majd XFSv5 szolgáltatások használatban van.
>


## <a name="support-for-network-configuration"></a>Hálózati konfiguráció támogatása
a következő táblák hello hálózati konfiguráció támogatja a különböző telepítési forgatókönyvek, amelyek használják az Azure Site Recovery tooreplicate tooAzure foglalják össze.

### <a name="host-network-configuration"></a>Gazdagép hálózati konfigurációja

**Konfigurálás** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a/nélkül a Virtual Machine Manager)**
--- | --- | ---
A hálózati adapterek összevonása | Igen<br/><br/>Nem támogatott, ha a fizikai gépeket replikálja a rendszer| Igen
VLAN | Igen | Igen
IPv4-alapú | Igen | Igen
IPv6 | Nem | Nem

### <a name="guest-vm-network-configuration"></a>Vendég virtuális gép hálózati konfigurációja

**Konfigurálás** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a/nélkül a Virtual Machine Manager)**
--- | --- | ---
A hálózati adapterek összevonása | Nem | Nem
IPv4-alapú | Igen | Igen
IPv6 | Nem | Nem
Statikus IP-címet (Windows) | Igen | Igen
Statikus IP-címet (Linux) | Igen <br/><br/>Virtuális gépek feladat-visszavétel a konfigurált toouse DHCP  | Nem
Több hálózati Adapterrel | Igen | Igen

### <a name="failed-over-azure-vm-network-configuration"></a>Átvevő Azure Virtuálisgép-hálózati konfiguráció

**Az Azure hálózatkezelés** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a/nélkül a Virtual Machine Manager)**
--- | --- | ---
Express Route | Igen | Igen
ILB | Igen | Igen
ÜZEMBE HELYEZETT ELB | Igen | Igen
Traffic Manager | Igen | Igen
Több hálózati Adapterrel | Igen | Igen
Fenntartott IP | Igen | Igen
IPv4-alapú | Igen | Igen
Tartsa meg a forrás IP-címe | Igen | Igen


## <a name="support-for-storage"></a>Tároló támogatása
a következő táblák hello tárolási konfiguráció támogatása a különböző telepítési forgatókönyvek, amelyek használják az Azure Site Recovery tooreplicate tooAzure foglalják össze.

### <a name="host-storage-configuration"></a>A gazdagép tároló konfigurálása

**Konfigurálás** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a/nélkül a Virtual Machine Manager)**
--- | --- | --- | ---
NFS | VMware Igen<br/><br/> Fizikai kiszolgálók esetében nem | N/A
SMB 3.0 | N/A | Igen
SAN (ISCSI) | Igen | Igen
(MPIO) többutas<br></br>A tesztelt: Microsoft DSM, az EMC PowerPath 5.7 SP4, az EMC PowerPath DSM CLARiiON a | Igen | Igen

### <a name="guest-or-physical-server-storage-configuration"></a>Vendég vagy fizikai tárolással

**Konfigurálás** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a/nélkül a Virtual Machine Manager)**
--- | --- | ---
VMDK-FÁJL | Igen | N/A
VHD/VHDX | N/A | Igen
Generációból 2 virtuális gép | N/A | Igen
EFI/UEFI| Nem | Igen
Megosztott fürtlemez | Nem | Nem
Titkosított lemez | Nem | Nem
NFS | Nem | N/A
SMB 3.0 | Nem | Nem
RDM | Igen<br/><br/> A fizikai kiszolgálók N/A | N/A
> 1 TB méretű lemez | Igen<br/><br/>Legfeljebb 4095 GB | Igen<br/><br/>Legfeljebb 4095 GB
4K szektorméretű lemez | Igen | Igen, a generáció 1 virtuális gépek esetén támogatott<br/><br/>Létrehozás 2 virtuális gépek esetén nem támogatott.
A csíkozott > 1 TB-os kötet<br/><br/> LVM logikai kötetkezelés | Igen | Igen
Tárolóhelyek | Nem | Igen
Gyakran használt adatok hozzáadása lemez | Nem | Nem
Lemez kizárása | Igen | Igen
(MPIO) többutas | N/A | Igen

**Azure Storage** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a/nélkül a Virtual Machine Manager)**
--- | --- | ---
LRS | Igen | Igen
GRS | Igen | Igen
RA-GRS | Igen | Igen
Ritkán használt adatok | Nem | Nem
Gyakran használt adatok| Nem | Nem
Rest(SSE) titkosítását| Igen | Igen
Prémium szintű Storage | Igen | Igen
Import/export szolgáltatás | Nem | Nem


## <a name="support-for-azure-compute-configuration"></a>Az Azure számítási konfigurációhoz támogatása

**Számítási szolgáltatás** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a/nélkül a Virtual Machine Manager)**
--- | --- | --- 
Rendelkezésre állási csoportok | Igen | Igen
HUB | Igen | Igen  
Felügyelt lemezek | Igen | Igen<br/><br/>Feladat-visszavétel tooon helyszíni Azure virtuális gépről az kezelt lemezek jelenleg nem támogatott.

## <a name="failed-over-azure-vm-requirements"></a>Átvevő Azure virtuális gép követelményei

A Site Recovery tooreplicate virtuális gépek és fizikai kiszolgálók Azure által támogatott operációs rendszert futtató is telepíthet. Ez a Windows és a Linux legtöbb verzióját magában foglalja. A helyszíni tooreplicate meg kell felelniük hello Azure-követelményeknek következő tooAzure replikálni kívánt virtuális gépeket.

**Entitás** | **Követelmények** | **Részletek**
--- | --- | ---
**Vendég operációs rendszer** | Hyper-V tooAzure replikáció: a Site Recovery minden operációs rendszereket támogatja [használható az Azure-](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> VMware és fizikai kiszolgáló replikációs: Ellenőrizze a hello Windows- és Linux [Előfeltételek](site-recovery-vmware-to-azure-classic.md) | Előfeltételek ellenőrzése sikertelen lesz, ha nem támogatott.
**Vendég operációs rendszer architektúrája** | 64 bites | Előfeltételek ellenőrzése sikertelen lesz, ha nem támogatott
**Operációs rendszert tároló lemez mérete** | Akár too2048 GB, ha replikál **VMware virtuális gépek vagy fizikai kiszolgálók tooAzure**.<br/><br/>Legfeljebb 2048 GB-ot **Hyper-V 1. generációs** virtuális gépeket.<br/><br/>Legfeljebb 300 GB-ot **Hyper-V 2. generációs virtuális gépek**.  | Előfeltételek ellenőrzése sikertelen lesz, ha nem támogatott
**Operációs rendszer lemez száma** | 1 | Előfeltételek ellenőrzése sikertelen lesz, ha nem támogatott.
**Adatlemez** | 64 vagy kevesebb if replikál **VMware virtuális gépek tooAzure**; 16 vagy kevesebb Ha replikál **Hyper-V virtuális gépek tooAzure** | Előfeltételek ellenőrzése sikertelen lesz, ha nem támogatott
**Adattároló lemez virtuális merevlemez mérete** | Másolatot too4095 GB | Előfeltételek ellenőrzése sikertelen lesz, ha nem támogatott
**Hálózati adapterek** | Több adapter támogatottak. |
**Megosztott virtuális merevlemez** | Nem támogatott | Előfeltételek ellenőrzése sikertelen lesz, ha nem támogatott
**FC-lemez** | Nem támogatott | Előfeltételek ellenőrzése sikertelen lesz, ha nem támogatott
**Merevlemez formátuma** | VIRTUÁLIS MEREVLEMEZ <br/><br/> VHDX | Bár VHDX jelenleg nem támogatott az Azure-ban, a Site Recovery automatikusan átalakítja VHDX tooVHD, amikor a rendszer átadja a tooAzure. Ha Ön a feladat-visszavételt a tooon helyszíni hello virtuális gépeket leállítaná toouse hello VHDX formátumú.
**A BitLocker** | Nem támogatott | A BitLocker a virtuális gépek védelme előtt le kell tiltani.
**Virtuális gép neve** | 1 és 63 karakter közötti. Korlátozott tooletters, számokat és kötőjeleket tartalmazhat. hello Virtuálisgép-nevet kell kezdődnie, és betűvel vagy számmal végződhet. | Frissítse a Site Recovery hello virtuálisgép-tulajdonságokat hello értéket.
**Virtuálisgép-típussá** | 1. generációs<br/><br/> Windows – a 2. generációs | 2. generációs virtuális gépek egy basic (amely egy vagy két adatkötetek VHDX formátumú tartalmazza) lemez típusa és kisebb, mint 300 GB lemezterület támogatottak.<br></br>Linux generációs 2 virtuális gépek nem támogatottak. [További információ](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="support-for-recovery-services-vault-actions"></a>Recovery Services-tároló műveletek támogatása

**Művelet** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (nincs Virtual Machine Managerrel)** | **A Hyper-V (Virtual Machine Managerrel)**
--- | --- | --- | ---
Erőforráscsoportok közötti áthelyezése közben tároló<br/><br/> Belül és között előfizetések | Nem | Nem | Nem
Tárolási, hálózati, Azure virtuális gépek között erőforráscsoportok áthelyezéséhez<br/><br/> Belül és között előfizetések | Nem | Nem | Nem


## <a name="support-for-provider-and-agent"></a>Provider és Agent támogatása

**Name (Név)** | **Leírás** | **Legújabb verzió** | **Részletek**
--- | --- | --- | --- | ---
**Az Azure Site Recovery Providert** | Koordinálja a helyszíni kiszolgálók és az Azure közötti kommunikáció <br/><br/> A helyi kiszolgálók, a Virtual Machine Manager vagy a Hyper-V kiszolgálók telepítve, ha nincs a Virtual Machine Manager-kiszolgáló | 5.1.19 ([elérhető portálról](http://aka.ms/downloaddra)) | [Legújabb funkcióit és javításokat](https://support.microsoft.com/kb/3155002)
**Az Azure Site Recovery egységes telepítője (VMware tooAzure)** | Koordinálja a helyszíni VMware-kiszolgálók és az Azure közötti kommunikáció <br/><br/> Helyszíni VMware-kiszolgálókon telepítve | 9.3.4246.1 (elérhető a portál) | [Legújabb funkcióit és javításokat](https://support.microsoft.com/kb/3155002)
**Mobilitási szolgáltatás** | Koordinálja a helyszíni VMware-kiszolgáló/fizikai kiszolgálók és az Azure és a másodlagos hely közötti replikálás<br/><br/> Szeretné telepíteni a VMware virtuális gép vagy fizikai kiszolgálók tooreplicate  | N/A (elérhető a portál) | N/A
**A Microsoft Azure Recovery Services (MARS) ügynök** | Koordinálja a Hyper-V virtuális gépek és az Azure közötti replikáció<br/><br/> Telepített helyszíni Hyper-V kiszolgálón (függetlenül a Virtual Machine Manager-kiszolgáló) | Legújabb ügynököt ([elérhető portálról](http://aka.ms/latestmarsagent)) |






## <a name="next-steps"></a>Következő lépések
[Előfeltételek ellenőrzése](site-recovery-prereq.md)
