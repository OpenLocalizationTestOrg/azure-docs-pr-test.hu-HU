---
title: "a Site Recovery szolgáltatással a Hyper-V virtuális gép replikációs aaaPlan hálózatleképezés |} Microsoft Docs"
description: "Állítsa be a hálózatra való leképezést a Hyper-V virtuális gépek replikációját egy helyszíni adatközpontban tooAzure vagy tooa másodlagos helyen."
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
ms.date: 05/23/2017
ms.author: raynew
ms.openlocfilehash: 86199b5840ea10fd33630bcc75d14340a49e01bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="plan-network-mapping-for-hyper-v-vm-replication-with-site-recovery"></a>A Site Recovery szolgáltatással a Hyper-V virtuális gép replikációs hálózatleképezés megtervezése



Ez a cikk segít toounderstand és felkészülés a Hyper-V virtuális gépek tooAzure, vagy a másodlagos hely tooa replikáció során leképezési, hello használata hálózati [Azure Site Recovery szolgáltatás](site-recovery-overview.md).

Olvasási után ez a cikk bármely fűzhetnek Ez a cikk hello alján, vagy technikai kérdései hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="network-mapping-for-replication-tooazure"></a>A replikációs tooAzure hálózatleképezés

A hálózatleképezés akkor használatos, ha a Hyper-V virtuális gépek (a VMM felügyelje) tooAzure replikálása. A hálózati forrás VMM-kiszolgálón a Virtuálisgép-hálózatok között, a hálózatleképezés kapcsolatot hoz létre, és a cél Azure-hálózatok. Leképezési hello a következő:

- **Hálózati kapcsolat**– biztosítja, hogy az replikált Azure virtuális gépekhez csatlakoztatott hálózati csatlakoztatott toohello. Feladatok átadása a hello azonos gépeire hálózati is csatlakozhat az egyéb tooeach, ha azokat a különböző helyreállítási tervben feladatátvételt.
- **Hálózati átjáró**– hello cél Azure-hálózatban hálózati átjáró be van állítva, ha a virtuális gépek kapcsolódhatnak tooother a helyszíni virtuális gépek.

Vegye figyelembe:

- Leképezheti a forrás VMM-Virtuálisgép-hálózat tooan Azure-beli virtuális hálózat.
- Hello Azure virtuális gépek feladatátvétel után a forrás hálózati csatlakoztatott toohello csatlakoztatott cél virtuális hálózati lesz.
- Új virtuális gépek hozzáadott toohello forrás Virtuálisgép-hálózathoz kapcsolódó replikáláskor toohello leképezve az Azure-hálózatot.
- Ha hello célhálózatban már több alhálózat működik, és ezek egyikének rendelkezik hello neve megegyezik a virtual machine hello forrás alhálózati található, akkor hello replika virtuális gép a feladatátvételt követően toothat cél alhálózathoz csatlakozik.
- Ha nincsenek egyező nevű cél alhálózathoz, hello virtuálisgép kapcsolódik toohello hello hálózat első alhálózatát.


## <a name="network-mapping-for-replication-tooa-secondary-datacenter"></a>A replikációs tooa másodlagos adatközpontba hálózatleképezés

A hálózatleképezés akkor használatos, ha a Hyper-V virtuális gépek (a System Center Virtual Machine Manager (VMM) a kezelt) tooa másodlagos adatközpontba replikálja. Hálózatleképezés kapcsolatot hoz létre a forrás VMM-kiszolgálón a Virtuálisgép-hálózatok és a célként megadott VMM-kiszolgálón a Virtuálisgép-hálózatok között. Leképezési hello a következő:

- **Hálózati kapcsolat**– csatlakoztatja a virtuális gépek tooappropriate hálózatok feladatátvételt követően. hello replika virtuális gép csatlakoztatott toohello cél hálózathoz csatlakoztatott toohello Forráshálózat lesz.
- **Optimális elhelyezési**– optimális helyek hello replika virtuális gépek a Hyper-V gazdakiszolgálókra. A replika virtuális gépeket, hogy hozzáférési hello rendelhetők a Virtuálisgép-hálózatok gazdagépeken kerülnek.
- **Nincs hálózati leképezés**– Ha nem adja meg a hálózatleképezés, a replika virtuális gépek a feladatátvételt követően nem lesz csatlakoztatott tooany Virtuálisgép-hálózatok.

Vegye figyelembe:

- A hálózatleképezés beállítható, hogy két VMM-kiszolgálókon a Virtuálisgép-hálózatok között, vagy egyetlen VMM-kiszolgálón, ha a két hely által felügyelt hello azonos kiszolgálón.
- Leképezési megfelelően van konfigurálva és replikáció engedélyezett-e, a virtuális gépek hello elsődleges helyen lesz csatlakoztatott tooa hálózati és fogja a replikáját hello célhelyen csatlakoznak tooits hálózati képezi le.
-
- Hálózatok rendelkezik megfelelően beállítva a VMM-ben a célként megadott Virtuálisgép-hálózat kiválasztásakor hálózatra való leképezés során, ha hello VMM forrás felhők hello forrás Virtuálisgép-hálózatot használó jelenik meg, hello az elérhető célkiszolgálók Virtuálisgép-hálózatok hello cél felhők használt együtt védelem.
- Ha hello célhálózatban már több alhálózat működik, és ezek egyikének ugyanazzal a névvel, mely hello a forrás virtuális gép is található, alhálózati hello majd hello hello a replika virtuális gép lesz a cél alhálózathoz csatlakoztatott toothat feladatátvételt követően. Ha nincsenek egyező nevű cél alhálózathoz, hello virtuális gép lesz a csatlakoztatott toohello hello hálózat első alhálózatát.



### <a name="example"></a>Példa

Íme egy példa tooillustrate a mechanizmus. A következőkben két helyen található Győr és Chicagói rendelkező szervezeteknél.

**Hely** | **VMM-kiszolgáló** | **A Virtuálisgép-hálózatok** | **Leképezve**
---|---|---|---
New York | A VMM-NewYork| VMNetwork1-NewYork | Csatlakoztatott tooVMNetwork1-Chicagói
 |  | VMNetwork2-NewYork | Nincsenek leképezve:
Chicago | A VMM-Chicagói| VMNetwork1-Chicagói | Csatlakoztatott tooVMNetwork1-NewYork
 | | VMNetwork1-Chicagói | Nincsenek leképezve:

Ebben a példában:

- A replika virtuális gép létrehozásakor bármely virtuális géphez, amely csatlakoztatott tooVMNetwork1-NewYork csatlakoztatott tooVMNetwork1-Chicagói lesz.
- Amikor a replika virtuális gép VMNetwork2-NewYork vagy VMNetwork2-Chicagói hoz létre, akkor nem fog csatlakoztatott tooany hálózati.

Ez hogyan VMM-felhő beállítása a szervezet példája és hello hello-felhőkhöz társított logikai hálózat.

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

Ezek a beállítások alapján hello cél Virtuálisgép-hálózat kiválasztásakor, hello a következő táblázatban látható hello lehetőségeket, amelyek elérhetők lesznek.

**Kiválasztás** | **Védett felhő** | **Felhő védelme** | **A célhálózat érhető el**
---|---|---|---
VMNetwork1-Chicagói | SilverCloud1 | SilverCloud2 | Elérhető
 | GoldCloud1 | GoldCloud2 | Elérhető
VMNetwork2-Chicagói | SilverCloud1 | SilverCloud2 | Nincs
 | GoldCloud1 | GoldCloud2 | Elérhető


Ha hello célhálózatban már több alhálózat működik, és ezek egyikének ugyanazzal a névvel, mely hello a forrás virtuális gép is található, alhálózati hello majd hello hello a replika virtuális gép lesz a cél alhálózathoz csatlakoztatott toothat feladatátvételt követően. Ha nincsenek egyező nevű cél alhálózathoz, hello virtuális gép lesz a csatlakoztatott toohello hello hálózat első alhálózatát.


#### <a name="failback-behavior"></a>Feladat-visszavétel viselkedése

Mi történik, a feladat-visszavétel (visszirányú replikálás), hello esetben toosee tegyük fel, hogy VMNetwork1-NewYork-e a csatlakoztatott a tooVMNetwork1-Chicago, a beállítások a következő hello rendelkező.


**Virtuális gép** | **Csatlakoztatott tooVM hálózati**
---|---
VM1 | VMNetwork1-hálózat
Vm2 virtuális gépnek (VM1 replika) | VMNetwork1-Chicagói

Ezekkel a beállításokkal tekintsük át, mi történik, több lehetséges forgatókönyv szerint.

**A forgatókönyv** | **Eredménye**
---|---
Nem módosult a feladatátvételt követően VM-2 hello hálózati tulajdonságait. | VM-1 csatlakoztatott toohello Forráshálózat marad.
VM-2 hálózati tulajdonságainak a feladatátvételt követően módosulnak, és le van választva. | VM-1 le van választva.
VM-2 hálózati tulajdonságainak a feladatátvételt követően módosulnak, és a csatlakoztatott tooVMNetwork2-Chicagói. | Ha nincs leképezve VMNetwork2-Chicago, VM-1 le lesz választva.
Hálózati leképezése VMNetwork1-Chicagói módosul. | VM-1 csatlakoztatott toohello most már csatlakoztatott hálózati tooVMNetwork1-Chicagói lesz.



## <a name="next-steps"></a>Következő lépések

További tudnivalók [hello hálózati infrastruktúrájának megtervezésével](site-recovery-network-design.md).
