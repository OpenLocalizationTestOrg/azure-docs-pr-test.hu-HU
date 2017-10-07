---
title: "a Hyper-V replikáció tooAzure System Center VMM aaaPrepare |} Microsoft Docs"
description: "Ismerteti, hogyan tooprepare System Center VMM-kiszolgáló a Hyper-V replikáció tooAzure Azure Site Recovery segítségével"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: afcd81ae-d192-4013-a0af-3dac45b3c7e9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 773b06afaf7d3eea1fe64f050bf3970943cf466a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-prepare-vmm-servers-and-hyper-v-hosts-for-hyper-v-replication-tooazure"></a>6. lépés: Felkészülés a Hyper-V replikáció tooAzure VMM-kiszolgáló és a Hyper-V-gazdagépek

Beállítása után [Azure összetevők](vmm-to-azure-walkthrough-prepare-azure.md) hello központi telepítéséhez használja a cikk tooprepare helyszíni VMM-kiszolgáló és a Hyper-V gazdagépek toointeract hello utasításait az Azure Site Recovery szolgáltatással.

A cikk elolvasása után bármely fűzhetnek hello lap alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-vmm-servers"></a>VMM-kiszolgáló előkészítése

- Meg kell legalább egy VMM-kiszolgálót a Site Recovery replikáció (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) hello támogatási követelményeknek.
- Győződjön meg arról, hogy előkészítése után a VMM-kiszolgáló hello [hálózatleképezés](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).
- Győződjön meg arról, hogy hello VMM-kiszolgáló el tudja érni az URL-címek

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- Ha IP-címeken alapuló tűzfalszabályok szabály, győződjön meg arról, kommunikációs tooAzure lehetővé teszik.
- Hello engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443) port hello.
- Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).

Site Recovery üzembe helyezése során hello Site Recovery Provider letöltése, és telepítse azt minden VMM-kiszolgálóhoz. a Recovery Services-tároló hello hello VMM-kiszolgáló regisztrálva.




## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[7. lépés: a tároló létrehozása](vmm-to-azure-walkthrough-create-vault.md)

