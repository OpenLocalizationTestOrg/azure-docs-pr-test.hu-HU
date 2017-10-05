---
title: "A replikálás az Azure Site Recovery egy másodlagos hely támogatási mátrix |} Microsoft Docs"
description: "A támogatott operációs rendszerek és összetevők összegzi az Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/24/2017
ms.author: raynew
ms.openlocfilehash: db7ee5251f2e2016081e55ca4b295e284c8b08cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="support-matrix-for-replication-to-a-secondary-site-with-azure-site-recovery"></a>A replikálás az Azure Site Recovery egy másodlagos hely támogatási mátrix

Ez a cikk összefoglalja, mit akkor támogatott, ha az Azure Site Recovery segítségével replikálása egy másodlagos helyszíni helyre.

## <a name="deployment-options"></a>Üzembe helyezési lehetőségek

**Üzembe helyezés** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a/SCVMM nélkül)**
--- | --- | --- | ---
**Azure Portal** | A helyszíni VMware virtuális gépek másodlagos VMware-hely.<br/><br/> Töltse le a [InMage Scout felhasználói útmutató](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) (az Azure portálon nem érhető el). | A helyszíni Hyper-V virtuális gépek VMM-felhőkben másodlagos VMM-felhőhöz.<br></br> A VMM nem támogatott  <br/><br/> Standard Hyper-V replikáció esetén. A SAN nem támogatott.
**Klasszikus portál** | Karbantartási mód csak. Nem hozható létre új tárolók. | Csak a karbantartási mód<br></br> Az SCVMM nem támogatott
**PowerShell** | Nem támogatott | Támogatott<br></br> Az SCVMM nem támogatott

## <a name="on-premises-servers"></a>A helyszíni kiszolgálók

### <a name="virtualization-servers"></a>Virtualizálási kiszolgálók

**Üzembe helyezés** | **Támogatás**
--- | ---
**VMware virtuális gép vagy fizikai kiszolgáló** | a vSphere 6.0, 5.5 vagy 5.1 legújabb frissítéssel
**A Hyper-V (a VMM-mel)** | A VMM 2016 és VMM 2012 R2

  >[!Note]
  > Windows Server 2016 és 2012 R2-állomások VMM 2016 felhők jelenleg nem támogatottak.

### <a name="host-servers"></a>Kiszolgálók

**Üzembe helyezés** | **Támogatás**
--- | ---
**VMware virtuális gép vagy fizikai kiszolgáló** | vCenter 5.5 vagy 6.0 (csak a 5.5 szolgáltatások támogatása)
**A Hyper-V (nincs VMM)** | Nem támogatott konfigurációkra vonatkozó replikálása egy másodlagos helyre
**A Hyper-V a VMM-mel** | Windows Server 2016 és a Windows Server 2012 R2 legújabb frissítéseit.<br/><br/> Windows Server 2016 gazdagépek a VMM 2016 kell kezelnie.

## <a name="support-for-replicated-machine-os-versions"></a>A replikált gép operációsrendszer-verziók támogatása
Az alábbi táblázat foglalja össze különböző telepítési forgatókönyvek észlelt, miközben az Azure Site Recovery segítségével az operációs rendszer támogatásának. Ez a támogatás nem alkalmazható az említett operációs rendszer bármilyen számítási feladatot.

**VMware vagy fizikai kiszolgáló** | **A Hyper-V (a VMM-mel)**
--- | --- | ---
64 bites Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2: legalább SP1<br/><br/> Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle 6.4 vagy 6.5 fut a Red Hat Enterprise Linux kompatibilis kernel vagy szoros vállalati Kernel Release 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 | A vendég operációs rendszer [Hyper-V által támogatott](https://technet.microsoft.com/library/mt126277.aspx)

>[!Note]
>A következő tárolási csak Linux gépek replikálhatja: fájlrendszer (EXT3, ETX4, ReiserFS, XFS); A többutas szoftver-eszköz leképező; Kötetkezelő (LVM2).
>HP CCISS vezérlő tároló fizikai kiszolgálók nem támogatottak.
>SUSE Linux Enterprise Server 11 SP3 csak támogatott ReiserFS fájlrendszert.

## <a name="network-configuration"></a>Hálózati konfiguráció

### <a name="hosts"></a>Hosts

**Konfigurálás** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a VMM-mel)**
--- | --- | ---
A hálózati adapterek összevonása | Igen | Igen
VLAN | Igen | Igen
IPv4-alapú | Igen | Igen
IPv6 | Nem | Nem

### <a name="guest-vms"></a>Vendég virtuális gépek

**Konfigurálás** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a VMM-mel)**
--- | --- | ---
A hálózati adapterek összevonása | Nem | Nem
IPv4-alapú | Igen | Igen
IPv6 | Nem | Nem
Statikus IP-címet (Windows) | Igen | Igen
Statikus IP-címet (Linux) | Igen | Igen
Több hálózati Adapterrel | Igen | Igen


## <a name="storage"></a>Storage

### <a name="host-storage"></a>Tárolók

**Tárhely (gazdagép)** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a VMM-mel)**
--- | --- | ---
NFS | Igen | N/A
SMB 3.0 | N/A | Igen
SAN (ISCSI) | Igen | Igen
(MPIO) többutas | Igen | Igen

### <a name="guest-or-physical-server-storage"></a>Vendég vagy fizikai kiszolgálón történő tárolás

**Konfigurálás** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a VMM-mel)**
--- | --- | ---
VMDK-FÁJL | Igen | N/A
VHD/VHDX | N/A | Igen (legfeljebb 16 lemez)
Generációból 2 virtuális gép | N/A | Igen
Megosztott fürtlemez | Igen  | Nem
Titkosított lemez | Nem | Nem
UEFI| Nem | N/A
NFS | Nem | Nem
SMB 3.0 | Nem | Nem
RDM | Igen | N/A
> 1 TB méretű lemez | Nem | Igen
A csíkozott > 1 TB-os kötet<br/><br/> LVM | Igen | Igen
Tárolóhelyek | Nem | Igen
Gyakran használt adatok hozzáadása lemez | Nem | Nem
Lemez kizárása | Nem | Igen
(MPIO) többutas | N/A | Igen

## <a name="vaults"></a>Tárolók

**Művelet** | **VMware vagy fizikai kiszolgáló** | **A Hyper-V (a VMM-mel)**
--- | --- | ---
Erőforráscsoportok (belül vagy között, előfizetések) közötti áthelyezése közben tárolók | Nem | Nem
Tárolási, hálózati, Azure virtuális gépek között (belül vagy között, előfizetések) erőforráscsoportok áthelyezéséhez | Nem | Nem

## <a name="provider-and-agent"></a>Provider és Agent

**Name (Név)** | **Leírás** | **Legújabb verzió** | **Részletek**
--- | --- | --- | --- | ---
**Az Azure Site Recovery Providert** | Koordinálja a helyszíni kiszolgálók és az Azure közötti kommunikáció <br/><br/> A helyszíni VMM-kiszolgálókon, vagy a Hyper-V kiszolgálókon telepítve, ha nincs VMM-kiszolgáló | 5.1.19 ([elérhető portálról](http://aka.ms/downloaddra)) | [Legújabb funkcióit és javításokat](https://support.microsoft.com/kb/3155002)
**Mobilitási szolgáltatás** | Koordinálja a helyszíni VMware Server vagy fizikai kiszolgálók és a másodlagos hely közötti replikálás<br/><br/> VMware virtuális gép vagy fizikai kiszolgálók replikálni kívánt telepítve  | N/A (elérhető a portál) | N/A


## <a name="next-steps"></a>Következő lépések

- [Hyper-V virtuális gépek VMM-felhőkben replikálása egy másodlagos helyre](site-recovery-vmm-to-vmm.md)
- [VMware-alapú virtuális gépek és fizikai kiszolgálók replikálása másodlagos helyre](site-recovery-vmware-to-vmware.md)
