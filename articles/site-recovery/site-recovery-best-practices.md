---
title: "aaaAzure Site Recovery gyakorlati tanácsok |} Microsoft Docs"
description: "A cikk az Azure Site Recovery üzembe helyezésének ajánlott eljárásait írja le"
services: site-recovery
documentationCenter: 
author: rayne-wiselman"
manager: cfreeman
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-support-matrix-to-azure
ms.openlocfilehash: 288df858a0e1c1f5ad96dbe8b9dd0dc69d8f56ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-ready-toodeploy-azure-site-recovery"></a>Készen áll a toodeploy Azure Site Recovery beolvasása

Ez a cikk ismerteti, hogyan tooprepare az Azure Site Recovery üzembe helyezése.

## <a name="protecting-hyper-v-virtual-machines"></a>Hyper-V virtuális gépek védelme

A Hyper-V virtuális gépek védelmére több üzembe helyezési lehetősége van. A helyszíni Hyper-virtuális gépek tooAzure vagy tooa másodlagos adatközpontba replikálhatja. Az egyes üzembe helyezéseknek eltérők a követelményei.

**Követelmény** | **(A VMM-mel) tooAzure replikálása** | **Replikálásához a Hyper-V virtuális gépek tooAzure (nincs VMM)** | **Replikálásához a Hyper-V virtuális gépek toosecondary hely (VMM)** | **Részletek**
---|---|---|---|---
**VMM** | System Center 2012 R2 rendszeren futó VMM <br/><br/>Legalább egy VMM-felhő, amely egy vagy több VMM-gazdagépcsoportot tartalmaz. | NA | VMM-kiszolgálókon futó legalább hello elsődleges és másodlagos helyek a System Center 2012 SP1 legújabb frissítéseit. <br/><br/> Legalább egy felhő minden VMM-kiszolgálón. Felhő beállítása a hello Hyper-V kapacitás profillal kell rendelkeznie.<br/><br/> hello forrásfelhőnek legalább egy VMM-gazdagépcsoportot kell rendelkeznie. | Választható. System Center VMM telepített rendelés tooreplicate Hyper-V virtuális gépek tooAzure toohave nem szükséges, de ellenkező esetben szüksége lesz a toomake meg arról, hogy a VMM-kiszolgáló hello megfelelően van beállítva. Amely tartalmazza a meggyőződött arról, hogy futtatja hello megfelelő a VMM verzióját, és a felhők vannak beállítva.
**Hyper-V** | Legalább egy Hyper-V gazdakiszolgáló a hello helyszíni hely Windows Server 2012 R2 rendszerű vagy újabb verzió | Legalább egy Hyper-V server rendszerű hello forrása és célja helyek hello legújabb frissítésekkel rendelkező Windows Server 2012 telepítve, és csatlakoztatva toohello internet.<br/><br/> hello Hyper-V kiszolgálók olyan gazdagépcsoportban egy VMM-felhőben kell lennie. | Legalább egy Hyper-V server rendszerű hello forrása és célja helyek hello legújabb frissítésekkel rendelkező Windows Server 2012 telepítve, és csatlakoztatva toohello internet.<br/><br/> hello Hyper-V kiszolgálók olyan gazdagépcsoportban egy VMM-felhőben kell lennie. |
**Virtuális gépek** | Legalább egy virtuális gép hello forrás Hyper-V gazdagép-kiszolgálón | Legalább egy virtuális gép hello Hyper-V gazdakiszolgáló a VMM-felhő hello forrás | Legalább egy virtuális gép hello Hyper-V gazdakiszolgáló a VMM-felhő hello forrás. |  Virtuális gépek replikálása tooAzure meg kell felelniük az Azure virtuális gép előfeltételei
**Azure-fiók** | Szüksége lesz egy Azure-fiókot és egy előfizetés toohello Site Recovery szolgáltatásban. | Szüksége lesz egy Azure-fiókot és egy előfizetés toohello Site Recovery szolgáltatásban. | NA | Ha nem rendelkezik fiókkal, kezdje az ingyenes próbaverzióval.
**Azure Storage tárterület** | Egy Azure Storage-fiókelőfizetésre lesz szüksége, amelyben engedélyezve van a georeplikáció. | Egy Azure Storage-fiókelőfizetésre lesz szüksége, amelyben engedélyezve van a georeplikáció. | NA | hello fióknak kell lennie a hello és hello Azure Site Recovery-tárolónak ugyanabban a régióban, és hello társítható ugyanahhoz az előfizetéshez.
**Hálózat** | Állítsa be, hogy az összes gép a feladatátvételt hello azonos Azure-hálózat hálózati leképezési tooensure tooeach más, a helyreállítási terv függetlenül is elérheti. Továbbá ha a hálózati átjáró hello cél Azure-hálózat van konfigurálva, a virtuális gépek kapcsolódhatnak tooother a helyszíni virtuális gépek. Ha nem állít be a hálózati leképezése csak azok a gépek hello kapcsolódhatnak azonos helyreállítási tervben feladatátvételt. | NA |  <br/><br/>Állítsa be a hálózati leképezési tooensure, amely a virtuális gépek a feladatátvételt követően csatlakoztatott tooappropriate-hálózatok találhatók, és hogy a replika virtuális gépek optimális kerülnek, a Hyper-V gazdakiszolgálókra. Ha nem konfigurálja a hálózati replikált gép leképezése nem lesz csatlakoztatott tooany Virtuálisgép-hálózat a feladatátvételt követően. |  fel a hálózatra való leképezést a VMM-mel tooset toomake meg arról, hogy a VMM logikai és a Virtuálisgép-hálózatok helyesen vannak konfigurálva lesz szüksége.
**Szolgáltatók és ügynökök** | Üzembe helyezése során telepítse hello Azure Site Recovery Providert a VMM-kiszolgálókon. A VMM-felhőkben Hyper-V kiszolgálókon telepítse hello Azure Recovery Services Agent ügynököt. | Üzembe helyezése során telepítse hello Azure Site Recovery Provider és a hello Azure Recovery Services Agent ügynököt a hello Hyper-V gazdakiszolgálón vagy fürtön| Üzembe helyezése során telepítse hello Azure Site Recovery Providert a VMM-kiszolgálókon. A VMM-felhőkben Hyper-V kiszolgálókon telepítse hello Azure Recovery Services Agent ügynököt. | Szolgáltatók és az ügynökök csatlakozás tooSite helyreállítási keresztül hello internetkapcsolat, és a titkosított HTTPS-kapcsolatot. Nem tooadd tűzfalkivételeket kell, vagy hozzon létre egy adott proxyt hello szolgáltató kapcsolathoz.
**Internetkapcsolat** | Csak hello VMM-kiszolgálókon internetkapcsolatra van szükségük. | Csak hello Hyper-V gazdakiszolgálók internetkapcsolatra van szükségük. | Csak VMM-kiszolgálóknak van szükségük internetkapcsolatra | A virtuális gépek telepítve rajtuk semmit nem kell, és ne csatlakozzon közvetlenül toohello internet.



## <a name="protect-vmware-virtual-machines-or-physical-servers"></a>A VMware virtuális gépek vagy fizikai kiszolgálók védelme

A VMware virtuális gépek, illetve Windows/Linux rendszerű kiszolgálók védelmére több üzembe helyezési lehetősége van. TooAzure vagy tooa másodlagos adatközpontba képes replikálni azokat. Az egyes üzembe helyezéseknek eltérők a követelményei.

**Követelmény** | **Replikáció VMware virtuális gépek/fizikai kiszolgálók tooAzure)** | * **VMware virtuális gépek/fizikai kiszolgálók toosecondary hely replikálása**  
---|---|---
**Elsődleges hely** | **Folyamatkiszolgáló**: egy dedikált Windows-kiszolgáló (fizikai vagy virtuális) | **Folyamatkiszolgáló**: egy dedikált Windows-kiszolgáló (fizikai vagy VMware virtuális gép<br/><br/>  
**Másodlagos helyszíni hely** | NA | **Konfigurációs kiszolgáló**: egy dedikált Windows-kiszolgáló (fizikai vagy virtuális) <br/><br/> **Fő célkiszolgáló**: egy dedikált kiszolgáló (fizikai vagy virtuális). Konfigurálja a Windows tooprotect Windows-alapú gépek vagy Linux tooprotect Linux.
**Azure** | **Előfizetés**: szüksége lesz egy előfizetés hello Site Recovery szolgáltatásban. <br/><br/> **Storage-fiók**: Egy Storage-fiókra lesz szüksége, amelyen engedélyezve van a georeplikáció. hello fióknak kell lennie a hello és hello Site Recovery-tárolónak ugyanabban a régióban, és hello társítható ugyanahhoz az előfizetéshez. <br/><br/> **Konfigurációs kiszolgáló**: szüksége lesz egy Azure virtuális gépként hello konfigurációs kiszolgáló tooset <br/><br/> **Fő célkiszolgáló**: hello fő kiszolgálót egy Azure virtuális gépként tooset lesz szüksége <br/><br/> Konfigurálja a Windows tooprotect Windows-alapú gépek vagy Linux tooprotect Linux.<br/><br/> **Azure-beli virtuális hálózat**: szüksége lesz egy Azure virtuális hálózatot, mely hello a konfigurációs kiszolgáló és a fő célkiszolgálón telepítve lesz. Nem lehet hello azonos előfizetésével és régiójával, hello Azure Site Recovery-tároló | NA  
**Virtuális gépek/fizikai kiszolgálók** | Legalább egy VMware virtuális gép vagy fizikai Windows-/Linux-kiszolgáló.<br/><br/>Központi telepítése során hello mobilitási szolgáltatás lesz telepítve minden gépen| Legalább egy VMware virtuális gép vagy fizikai Windows-/Linux-kiszolgáló.<br/><br/> Az egyes központi telepítése során hello egyesített ügynök van telepítve.




## <a name="azure-virtual-machine-requirements"></a>Az Azure virtuális gépek követelményei

A Site Recovery tooreplicate virtuális gépek és fizikai kiszolgálók Azure által támogatott operációs rendszert futtató is telepíthet. Ez a Windows és a Linux legtöbb verzióját magában foglalja. Szükség lesz toomake meg arról, hogy a helyszíni virtuális gépek, amelyet az tooprotect adott a specifikációknak való Azure-követelményeknek.


## <a name="optimizing-your-deployment"></a>Az üzemelő példány optimalizálása

A következő tippek toohelp optimalizálása és a telepítés méretezésére hello használata.

- **Az operációsrendszer-kötet mérete**: 1 Terabájtnál kisebb mikor replikálja a virtuális gép tooAzure hello rendszerkötetben működő kell lennie. Ha több kötet ezt manuálisan is helyezze azokat tooa másik lemezt telepítés megkezdése előtt.
- **Adatok lemezméret**: Ha replikál tooAzure too32 adatok lemezeket a virtuális gépen, egyenként legfeljebb 1 TB-os lehet. Gyakorlatilag legfeljebb ~32 TB méretű virtuális gépek replikálását és feladatátvételét végezheti el.
- **Helyreállítási terv korlátok**: a Site Recovery toothousands a virtuális gépek méretezhető. A helyreállítási terv modellként kell feladataikat együtt átadó, a helyreállítási terv too50 gépek hello számának korlátozása a Microsoft-alkalmazások úgy lettek kialakítva.
- **Az Azure szolgáltatási korlátai**: Minden Azure-előfizetésre alapértelmezett korlátok vonatkoznak a magok, felhőszolgáltatások stb. tekintetében. Azt javasoljuk, hogy a teszt feladatátvételi toovalidate hello rendelkezésre állását erőforrások az előfizetésben. Ezeket a korlátokat az Azure ügyfélszolgálatánál módosíthatja.
- **Kapacitástervezés**: Tervezzen a méretezésre és a teljesítményre.
- **Replikációs sávszélesség**: Ha alacsony a replikációs sávszélessége, vegye figyelemre a következőket:
    - **ExpressRoute**: A Site Recovery működik az Azure ExpressRoute- és WAN-optimalizálókkal, például a Riverbed szolgáltatással.
    - **Replikációs forgalom**: a Site Recovery által használt egy intelligens kezdeti replikáció csak az adatblokkokat használatával hajtja végre, és nem hello teljes virtuális Merevlemezt. A következő replikációk során a rendszer csak a módosításokat replikálja.
    - **Hálózati forgalom**: hello cél IP-cím és port alapján házirendnek beállításával Windows QoS-replikációhoz használt hálózati forgalom szabályozhatja.  Továbbá ha a Site Recovery tooAzure replikál hello Azure Backup-ügynök használatával. konfigurálhatja a szabályozást az ügynökhöz.
- **RTO**: Ha azt szeretné, hogy toomeasure hello helyreállítási idő célkitűzése (RTO) a Site Recovery számíthat javasoljuk, hogy feladatátvételi teszt futtatása és a nézet a Site Recovery-feladatok tooanalyze hello mennyi időt vesz igénybe toocomplete hello műveletek. Ha a feladat-visszavétele tooAzure keresztül, a hello legjobb RTO azt javasoljuk, hogy az összes manuális műveletek integrálása az Azure Automation szolgáltatásbeli és helyreállítási automatizálhatja tervezi.
- **A helyreállítási Időkorlát**: a Site Recovery támogatja közel egyidejű helyreállítási időkorlát (RPO) tooAzure a replikált. Ez a módszer elegendően nagy sávszélességet feltételez az adatközpont és az Azure között.
