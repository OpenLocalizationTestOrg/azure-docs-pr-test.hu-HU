---
title: "A Site Recovery szolgáltatással a Hyper-V virtuális gép replikációs hálózatleképezés megtervezése |} Microsoft Docs"
description: "Állítsa be a hálózatra való leképezést a Hyper-V virtuális gépek replikációját egy helyszíni adatközpontot az Azure-bA vagy másodlagos helyhez."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: tysonn
ms.assetid: fcaa2f52-489d-4c1c-865f-9e78e000b351
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/30/2017
ms.author: raynew
ms.openlocfilehash: 91d6d0466789daa662162c60bc3c97ba6115e7eb
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/01/2017
---
# <a name="plan-network-mapping-for-hyper-v-vm-replication-with-site-recovery"></a>A Site Recovery szolgáltatással a Hyper-V virtuális gép replikációs hálózatleképezés megtervezése



Ez a cikk segít megismernie és megterveznie hálózati leképezése során a Hyper-V virtuális gépek Azure-bA vagy másodlagos helyre replikációt, használja a [Azure Site Recovery szolgáltatás](site-recovery-overview.md).

Olvasási után ez a cikk a megjegyzéseket, ez a cikk alján, vagy technikai kérdéseket a a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="network-mapping-for-replication-to-azure"></a>A replikáció az Azure hálózatleképezés

A hálózatleképezés használatos (a VMM felügyelje) Hyper-V virtuális gépek replikálása az Azure-bA. A hálózati forrás VMM-kiszolgálón a Virtuálisgép-hálózatok között, a hálózatleképezés kapcsolatot hoz létre, és a cél Azure-hálózatok. Leképezés a következőket teszi:

- **Hálózati kapcsolat**– biztosítja, hogy a replikált Azure virtuális gépek csatlakoznak-e a csatlakoztatott hálózati. Minden olyan gép, amelyek ugyanazon a hálózaton keresztül nem lehet csatlakozni egymáshoz, akkor is, ha azokat a különböző helyreállítási tervben feladatátvételt.
- **Hálózati átjáró**– Ha a cél Azure-hálózatban hálózati átjáró be van állítva, a virtuális gépek más helyszíni virtuális gépek csatlakozhat.

Vegye figyelembe:

- A forrás VMM-Virtuálisgép-hálózathoz csatlakoztat egy Azure virtuális hálózatra.
- A forrás Azure virtuális gépek a feladatátvételt követően hálózati fog csatlakozni a csatlakoztatott virtuális célhálózat.
- A forrás Virtuálisgép-hálózathoz hozzáadott új virtuális gépek replikáláskor csatlakoznak a leképezett Azure-hálózathoz.
- Ha a célhálózatban már több alhálózat működik, és ezek egyikének ugyanaz a neve, mint annak, amelyen a forrás virtuális gép is található, a replika virtuális gép a feladatátvételt követően ehhez a cél alhálózathoz fog csatlakozni.
- Ha nem létezik egyező nevű alhálózat, a virtuális gép a hálózat első virtuális alhálózatához csatlakozik.


## <a name="network-mapping-for-replication-to-a-secondary-datacenter"></a>A hálózatleképezés replikáció egy másodlagos adatközpontba

A hálózatleképezés (a System Center Virtual Machine Manager (VMM) a kezelt) Hyper-V virtuális gépek replikálása másodlagos adatközpontba használatos. Hálózatleképezés kapcsolatot hoz létre a forrás VMM-kiszolgálón a Virtuálisgép-hálózatok és a célként megadott VMM-kiszolgálón a Virtuálisgép-hálózatok között. Leképezés a következőket teszi:

- **Hálózati kapcsolat**– kapcsolódó virtuális gépek a feladatátvételt követően a megfelelő hálózatokhoz. A replika virtuális gép csatlakoznak-e a cél hálózati egy forrásoldali hálózatot csatlakoztatott.
- **Optimális elhelyezési**– optimális helyezi el a replika virtuális gépek a Hyper-V gazdakiszolgálókra. A replika virtuális gépek gazdagépek férhetnek hozzá a csatlakoztatott Virtuálisgép-hálózatok kerül.
- **Nincs hálózati leképezés**– Ha nem adja meg a hálózatleképezés, a replika virtuális gépek nem csatlakozik Virtuálisgép-hálózatok a feladatátvételt követően.

Vegye figyelembe:

- A hálózatleképezés konfigurálható két VMM-kiszolgálókon, vagy a VMM-kiszolgálón egy egyetlen Ha két hely ugyanarra a kiszolgálóra által kezelt Virtuálisgép-hálózatok között.
- Ha leképezést megfelelően van konfigurálva és engedélyezve van a replikáció, az elsődleges helyen a virtuális gépek csatlakoznak-e a hálózati és fogja a replikáját a célhelyen csatlakoznak-e a csatlakoztatott hálózati.
-
- Hálózatok rendelkezik megfelelően beállítva a VMM-ben a célként megadott Virtuálisgép-hálózat kiválasztásakor hálózatra való leképezés során, ha a forrás Virtuálisgép-hálózatot használó VMM forrás felhők jelenik meg, az elérhető célkiszolgálók Virtuálisgép-hálózatok a cél felhők, amely a védelemhez használt együtt.
- Ha a célhálózatban már több alhálózat működik, és ezek egyikének a neve megegyezik az alhálózat, amelyen a forrás virtuális gép is található, majd a replika virtuális géphez csatlakoznak a cél alhálózathoz feladatátvételt követően. Ha nem létezik egyező nevű alhálózat, a virtuális gép a hálózat első virtuális gépéhez csatlakozik.



### <a name="example"></a>Példa

Íme egy példa a mechanizmus mutatja be. A következőkben két helyen található Győr és Chicagói rendelkező szervezeteknél.

**Hely** | **VMM-kiszolgáló** | **A Virtuálisgép-hálózatok** | **Leképezve**
---|---|---|---
New York | A VMM-NewYork| VMNetwork1-NewYork | VMNetwork1-Chicagói leképezve
 |  | VMNetwork2-NewYork | Nincsenek leképezve:
Chicago | A VMM-Chicagói| VMNetwork1-Chicagói | VMNetwork1-NewYork leképezve
 | | VMNetwork1-Chicagói | Nincsenek leképezve:

Ebben a példában:

- A replika virtuális gép létrehozásakor bármely virtuális géphez csatlakoztatott VMNetwork1-NewYork, VMNetwork1-Chicagóba csatlakoznak.
- Amikor a replika virtuális gép VMNetwork2-NewYork vagy VMNetwork2-Chicagói hoz létre, azt nem csatlakoznak a hálózathoz.

Ez hogyan VMM-felhő beállítása a szervezet példája, és a logikai hálózatok felhőkhöz van társítva.

#### <a name="cloud-protection-settings"></a>Felhő védelmi beállításait

**Védett felhő** | **Felhő védelme** | **Logikai hálózat (New Yorkban)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicagói</p>
SilverCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicagói</p>

#### <a name="logical-and-vm-network-settings"></a>Logikai és a virtuális gép hálózati beállításai

**Hely** | **Logikai hálózat** | **Társított Virtuálisgép-hálózat**
---|---|---
New York | LogicalNetwork1-NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1-Chicagói | VMNetwork1-Chicagói
 | LogicalNetwork2Chicago | VMNetwork2-Chicagói

#### <a name="target-network-settings"></a>Cél hálózati beállításai

Ezek a beállítások alapján a célként megadott Virtuálisgép-hálózat kiválasztásakor, az alábbi táblázat a lehetőségeket, amelyek elérhetők lesznek.

**Kiválasztás** | **Védett felhő** | **Felhő védelme** | **A célhálózat érhető el**
---|---|---|---
VMNetwork1-Chicagói | SilverCloud1 | SilverCloud2 | Elérhető
 | GoldCloud1 | GoldCloud2 | Elérhető
VMNetwork2-Chicagói | SilverCloud1 | SilverCloud2 | Nincs
 | GoldCloud1 | GoldCloud2 | Elérhető


Ha a célhálózatban már több alhálózat működik, és ezek egyikének a neve megegyezik az alhálózat, amelyen a forrás virtuális gép is található, majd a replika virtuális géphez csatlakoznak a cél alhálózathoz feladatátvételt követően. Ha nem létezik egyező nevű alhálózat, a virtuális gép a hálózat első virtuális gépéhez csatlakozik.


#### <a name="failback-behavior"></a>Feladat-visszavétel viselkedése

Mi történik, a feladat-visszavétel (visszirányú replikálás) esetén megtekintéséhez tegyük fel, hogy VMNetwork1-NewYork van leképezve VMNetwork1-Chicago, a következő beállításokkal.


**Virtuális gép** | **Virtuálisgép-hálózathoz csatlakozik**
---|---
VM1 | VMNetwork1-hálózat
Vm2 virtuális gépnek (VM1 replika) | VMNetwork1-Chicagói

Ezekkel a beállításokkal tekintsük át, mi történik, több lehetséges forgatókönyv szerint.

**A forgatókönyv** | **Eredménye**
---|---
A feladatátvételt követően VM-2 hálózati tulajdonságok nem módosult. | VM-1 a forrás hálózati kapcsolatban marad.
VM-2 hálózati tulajdonságainak a feladatátvételt követően módosulnak, és le van választva. | VM-1 le van választva.
VM-2 hálózati tulajdonságainak a feladatátvételt követően módosulnak, és VMNetwork2-Chicagói csatlakozik. | Ha nincs leképezve VMNetwork2-Chicago, VM-1 le lesz választva.
Hálózati leképezése VMNetwork1-Chicagói módosul. | VM-1 VMNetwork1-Chicagói most leképezve a hálózathoz csatlakoznak.



## <a name="next-steps"></a>Következő lépések

További tudnivalók [a hálózati infrastruktúra tervezési](site-recovery-network-design.md).
