---
title: aaaWhat az Azure Site Recovery? | Microsoft Docs
description: "Hello Azure Site Recovery szolgáltatás áttekintése, és a központi telepítési forgatókönyvek foglalja össze."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: e9b97b00-0c92-4970-ae92-5166a4d43b68
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/25/2017
ms.author: raynew
ms.openlocfilehash: da6755654b8036a03314ec836f014b64428d5518
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-site-recovery"></a>Mi a Site Recovery?

Üdvözli a toohello Azure Site Recovery szolgáltatás! Ez a cikk gyors áttekintést nyújt az hello szolgáltatást.

## <a name="business-continuity-and-disaster-recovery-bcdr-with-azure-recovery-services"></a>Üzletmenet-folytonosság és vészhelyreállítás (BCDR) az Azure Recovery Services használatával

Szervezeti toofigure ki, hogyan fog tookeep a biztonságos adatokat kell, és tervezett futó alkalmazások vagy munkaterhelések és a nem tervezett leállások történik.

Az Azure Recovery Services tooyour BCDR stratégia közreműködhet:

- **Site Recovery szolgáltatás**: A Site Recovery azzal segít fenntartani az üzletmenet folytonosságát, hogy egy hely leállása esetén tovább futtatja az alkalmazásokat az elérhető virtuális gépeken és fizikai kiszolgálókon. A Site Recovery replikálja, hogy elérhetők maradjanak a másodlagos helyen hello elsődleges hely nem érhető el a virtuális gépek és fizikai kiszolgálókon futó számítási feladatok. Azt állítja helyre a munkaterhelések toohello elsődleges helyhez, működik-e, majd futtassa újra.
- **Biztonsági mentési szolgáltatás**: emellett hello [Azure biztonsági mentés](https://docs.microsoft.com/azure/backup/) szolgáltatás biztosítja az adatok biztonságáról és helyreállíthatóságáról által a biztonsági mentés tooAzure.

A Site Recovery a következők replikációját képes kezelni:

- Azure virtuális gépek replikációja Azure-régiók között.
- A helyszíni virtuális gépek és fizikai kiszolgálók replikálása tooAzure vagy tooa másodlagos hely.


## <a name="what-does-site-recovery-provide"></a>Mit nyújt a Site Recovery?

**Funkció** | **Részletek**
--- | ---
**Egyszerű BCDR-megoldás üzembe helyezése** | A Site Recovery segítségével, állíthat be, és replikációjának, feladatátvételének és feladat-visszavétel hello Azure-portálon a egyetlen helyről kezelheti.
**Azure-beli virtuális gépek replikálása** | A BCDR-stratégia beállítható úgy, hogy az Azure virtuális gépek Azure-régiók között replikálódjanak.
**Helyszíni virtuális gépek telephelyen kívüli replikációja** | A helyszíni virtuális gépek és fizikai kiszolgálók tooAzure vagy tooa másodlagos helyszíni helyre replikálhatja. Replikációs tooAzure hello költségek és egy másodlagos adatközpontba fenntartásának megoldással.
**Bármilyen számítási feladat replikálható** | A támogatott Azure virtuális gépeken, helyszíni Hyper-V-alapú virtuális gépeken, a VMware-alapú virtuális gépeken és a Windows-/Linux-alapú fizikai kiszolgálókon futó bármilyen számítási feladat replikálható.
**Adatok biztonságos, rugalmas megőrzése** | A Site Recovery koordinálja a replikációt, de az alkalmazás adataihoz nem fér hozzá. A replikált adatok az Azure storage hello általa biztosított tárolt. Ha feladatátvétel történik, Azure virtuális gépek hello replikált adatok alapján jönnek létre.
**RTO és RPO** | A helyreállítási idők célkitűzéseinek (recovery time objectives, RTO-k) és a helyreállítási pont célkitűzéseinek (recovery point objectives, RPO-k) a szervezeti korlátokon belül tartása. A Site Recovery folyamatos replikációt biztosít Azure és VMware virtuális gépek esetén, és csupán 30 másodperces replikációs frekvenciát Hyper-V esetén. A helyreállítási idők célkitűzései tovább csökkenthetők az [Azure Traffic Manager](https://azure.microsoft.com/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/)rel való integrációval.
**Alkalmazás-konzisztencia feladatátvétel esetén** | Helyreállítási pontok konfigurálhatók alkalmazás-konzisztens pillanatképekkel. Az alkalmazáskonzisztens pillanatképek a lemez adatait, a memóriában lévő összes adatot és az összes folyamatban lévő tranzakciót is rögzítik.
**Megszakítás nélküli teszt** | Könnyen futtathatja teszteket toosupport vészhelyreállítások részletezésének, folyamatban lévő replikáció befolyásolása nélkül.
**Rugalmas feladatátadások futtatása** | Nulla adatvesztéssel járó tervezett feladatátvételeket is futtathat várt leállások esetére, illetve (a replikáció gyakoriságától függően) minimális adatvesztéssel járó nem tervezett feladatátvételeket a váratlan vészhelyzetek esetére. Hátsó tooyour elsődleges hely könnyen meghiúsulhatnak, ha azok elérhetők újra.
**Helyreállítási tervek létrehozása** | Helyreállítási tervekkel több virtuális gépen futó többszintű alkalmazások feladatátvételének és helyreállításának műveletsora is egyénileg kialakítható. Elvégezheti a gépek terveken belüli csoportosítását, valamint szkripteket és manuális műveleteket vehet fel. A helyreállítási tervek integrálhatók az Azure Automation-runbook használatával.
**Integráció meglévő BCDR-technológiákkal** | A Site Recovery más BCDR-technológiákkal integrálható. Például a Site Recovery tooprotect hello SQL Server háttér a vállalati számítási feladatok, beleértve az SQL Server AlwaysOn rendelkezésre állási csoportok feladatátvételének toomanage hello natív támogatását is használhatja.
**Hello automatizálási könyvtár integrálása** | Az Azure Automation-könyvtár gazdag, éles használatra kész és alkalmazásspecifikus parancsfájlokat tartalmazó automatizálási könyvtár, amely letölthető, és beépíthető a Site Recovery szolgáltatásba.
**Hálózati beállítások kezelése** | A Site Recovery és az Azure integrációja leegyszerűsíti az alkalmazáshálózat kezelését, ideértve az IP-címek lefoglalását, a terheléselosztók konfigurálását, valamint az Azure Traffic Manager integrációját, ami hatékony hálózatváltást garantál.


## <a name="what-can-i-replicate"></a>Miket replikálhatok?

**Támogatott** | **Részletek**
--- | ---
**Miket replikálhatok?** | Azure virtuális gépek Azure-régiók között (előzetes)<br/><br/>  Helyszíni VMware virtuális gépek, a Hyper-V virtuális gépek, fizikai kiszolgálókon (a Windows és Linux) tooAzure < br /<br/> Helyszíni VMware virtuális gépek, a Hyper-V virtuális gépek, fizikai kiszolgálók tooa másodlagos helyen. Hyper-V virtuális gépek replikációs tooa másodlagos helyen csak támogatott, ha a System Center VMM által felügyelt Hyper-V-gazdagépek.
**Mely régiók támogatottak a Site Recoveryhez?** | [Támogatott régiók](https://azure.microsoft.com/regions/services/) |
**Mely operációs rendszerek használhatóak a replikált gépeken?** | [Azure virtuálisgép-követelmények](site-recovery-support-matrix-azure-to-azure.md#support-for-replicated-machine-os-versions)<br></br>[VMware virtuálisgép-követelmények](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)<br/><br/> A Hyper-V virtuális gépek esetében az Azure által támogatott bármely [vendég operációs rendszer](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) és a Hyper-V is támogatott.<br/><br/> [Fizikai kiszolgáló követelmények](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)
**Milyen VMware-kiszolgálókra/-gazdagépekre van szükségem?** | VMware virtuális gépek elhelyezhetők [támogatott vSphere gazdagépeken/vCenter kiszolgálókon](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers)
**Milyen számítási feladatokat replikálhatok?** | A támogatott replikációs gépeken futó bármilyen számítási feladat replikálható. Ezenkívül hello Site Recovery csapat által végrehajtott alkalmazásspecifikus teszteket a egy [alkalmazások száma](site-recovery-workload.md#workload-summary).


## <a name="azure-portal-considerations"></a>Azure Portal tudnivalók

* A Site Recovery üzembe helyezhető hello [Azure-portálon](https://portal.azure.com).
* Hello a klasszikus Azure portálon, a Site Recovery segítségével kezelheti az hello klasszikus szolgáltatások felügyeleti modellt.
- hello klasszikus portál csak kell használt toomaintain Site Recovery telepítéseit. Nem hozható létre új tárolók hello a klasszikus portálon.

## <a name="next-steps"></a>Következő lépések
* További információk a [támogatott számítási feladatokról](site-recovery-workload.md)
* Ismerkedés a [Azure virtuális gép replikációs régiók közötti](site-recovery-azure-to-azure.md), [VMware replikációs tooAzure](vmware-walkthrough-overview.md), vagy [Hyper-V replikáció tooAzure](hyper-v-site-walkthrough-overview.md).
