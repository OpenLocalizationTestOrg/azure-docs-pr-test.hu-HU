---
title: "Hyper-V tooAzure replikációs (a System Center VMM-mel) Azure Site Recovery segítségével aaaReview hello előfeltételei |} Microsoft Docs"
description: "Ismerteti a replikálása, feladatátvétele és az Azure Site Recovery a helyszíni Hyper-V virtuális gépek VMM-felhők tooAzure, a helyreállítási beállítása hello előfeltételei"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a1c30fd5-c979-473c-af44-4f725ad3e3ba
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: raynew
ms.openlocfilehash: de13a2d80b1a9a5d968a180d559f661ab11e70c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-for-hyper-v-with-vmm-tooazure-replication"></a>2. lépés: Hyper-V (a VMM-mel) tooAzure replikációs hello előfeltételeinek áttekintése

Most áttekintése után hello [kialakítandó architektúra](vmm-to-azure-walkthrough-architecture.md), olvassa el a cikk toomake tisztában hello telepítésének előfeltételei. 

## <a name="prerequisites-and-limitations"></a>Előfeltételek és korlátozások

**Követelmény** | **Részletek**
--- | ---
**Azure-fiók** | Szüksége van egy [Microsoft Azure-fiókra](http://azure.microsoft.com/).
**Azure Storage** | Szüksége van egy Azure storage fiók toostore replikált adatok.<br/><br/> hello tárfiókot kell hello megegyező régióban hello Azure Recovery Services-tároló.<br/><br/>[Georedundáns tárolást](../storage/common/storage-redundancy.md#geo-redundant-storage) és helyileg redundáns tárolást is használhat. A georedundáns tárolást javasoljuk. Georedundáns tárolás az adatokat is lehetséges regionális kimaradás során, vagy ha az elsődleges régióban hello nem állíthatók vissza.<br/><br/> Standard szintű Azure Storage-tárfiókot vagy Azure [Prémium szintű tárolást](../storage/common/storage-premium-storage.md) is használhat. A Prémium szintű tárolás képes I/O-igényes számítási feladatok futtatására, és általában olyan virtuális gépekhez használják, amelyek számára elengedhetetlen a folyamatosan magas I/O-teljesítmény és a kis késés. Ha Prémium szintű tárolást használ a replikált adatokhoz, Standard szintű tárfiókra is szüksége van. Standard szintű tárfiókot a replikálási naplókhoz, folyamatban lévő változtatások tooon helyszíni adatok rögzítéséhez tárolja.
**Azure-hálózat** | Kell egy [Azure hálózati](../virtual-network/virtual-network-get-started-vnet-subnet.md), toowhich Azure virtuális gépek a feladatátvételt követően csatlakozhatnak. hello Azure hálózat kell hello hello és ugyanabban a régióban Recovery Services-tároló.
**Helyszíni VMM-kiszolgálók** | Egy vagy több, a System Center 2012 R2 vagy újabb verziójával futó VMM-kiszolgálóra van szüksége.<br/><br/> Minden VMM-kiszolgálón legalább egy magánfelhőnek kell üzemelnie. Mindegyik felhőben legalább egy gazdagépcsoportnak kell lennie.<br/><br/> hello VMM-kiszolgáló internetes hozzáférésre van szükség.
**Helyszíni Hyper-V** | Hyper-V gazdakiszolgálókon legalább futnia kell a Windows Server 2012 R2 hello Hyper-V szerepkör engedélyezve van, vagy a Microsoft Hyper-V Server 2012 R2. hello legújabb frissítéseket kell telepíteni.<br/><br/> hello Hyper-V gazdagép egy VMM-gazdagépcsoport (a VMM-felhőben lévő) kell elhelyezni.<br/><br/> A gazdagép egy vagy több virtuális gépet, amelyet az tooreplicated kell rendelkeznie.<br/><br/> Hyper-V-gazdagépeken kell lennie a csatlakoztatott toohello internet replikációs tooAzure, közvetlenül vagy egy proxy. Hyper-V-kiszolgálóknak rendelkezniük kell a cikkben ismertetett hello javítások [2961977](https://support.microsoft.com/kb/2961977).
**Helyszíni Hyper-V rendszerű virtuális gépek** | Virtuális gépek kívánt tooreplicate rendszerűnek kell lennie egy [támogatott operációs rendszer](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), és adott a specifikációknak való [Azure-Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). hello virtuális gép neve után a replikáció engedélyezett-e módosítható. 




## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[3. lépés: kapacitástervezés](vmm-to-azure-walkthrough-capacity.md)
