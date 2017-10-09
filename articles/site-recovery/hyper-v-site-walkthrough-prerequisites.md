---
title: "Hyper-V tooAzure replikációs (a System Center VMM nélkül) Azure Site Recovery segítségével aaaReview hello előfeltételei |} Microsoft Docs"
description: "Ismerteti a replikálása, feladatátvétele és helyreállítása az Azure Site Recovery a helyszíni Hyper-V virtuális gépek tooAzure beállítása hello előfeltételei"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 7ef3fb46-52f5-4c8a-b1a1-658c2305762a
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 3eefc3b7e3982ec6c413c1db7f7784863f9c701d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-for-hyper-v-without-vmm-tooazure-replication"></a>2. lépés: Hyper-V (VMM nélkül) tooAzure replikációs hello előfeltételeinek áttekintése

hello Előfeltételek hello táblázat foglalja össze.


**Előfeltétel** | **Részletek** 
--- | --- 
**Azure** | Az [Azure-követelmények](site-recovery-prereq.md#azure-requirements) ismertetése.
**Helyszíni kiszolgálók** | [További](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm) hello helyszíni Hyper-V-gazdagépek követelményeivel kapcsolatos.
**Helyszíni Hyper-V rendszerű virtuális gépek** | Virtuális gépek kívánt tooreplicate rendszerűnek kell lennie egy [támogatott operációs rendszer](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), és adott a specifikációknak való [Azure-Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Azure-beli URL-címek** | Hyper-V-gazdagépek toothese URL-címeket kell elérni:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Ha IP-címeken alapuló tűzfalszabályok szabály, győződjön meg arról, kommunikációs tooAzure lehetővé teszik.<br/></br> Hello engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443) port hello.<br/></br> Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).



## <a name="next-steps"></a>Következő lépések

- Ha teljes körű telepítésére végzett, nyissa meg túl[3. lépés: kapacitástervezés](hyper-v-site-walkthrough-capacity.md)
- Ha egy egyszerű próbatelepítés végzett, nyissa meg túl[4. lépés: hálózati terv](hyper-v-site-walkthrough-network.md).
