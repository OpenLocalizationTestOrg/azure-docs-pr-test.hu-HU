---
title: "aaaPrepare Hyper-V gazdagépek (a System Center VMM nélkül) a replikációs tooAzure |} Microsoft Docs"
description: "Ismerteti, hogyan tooprepare Hyper-V gazdagépek, az Azure Site Recovery segítségével replikációs tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0f204e24-8d78-4076-95c5-8137d1be9c01
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 714b229d5efbd66a9844bd09e36ac3f69919a6bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-prepare-hyper-v-hosts-for-replication-tooazure"></a>6. lépés: Hyper-V-gazdagépek előkészítése replikációs tooAzure

Ez a cikk tooprepare hello utasításait használatát a helyszíni Hyper-V gazdagépek toointeract az Azure Site Recovery szolgáltatással.

A cikk elolvasása után bármely fűzhetnek hello lap alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-hosts"></a>Gazdagépek előkészítése

- Győződjön meg arról, hogy hello Hyper-V gazdagépek megfelelnek-e a hello [Előfeltételek](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).
- Győződjön meg arról, hogy a hello állomások hozzáférhet-e a szükséges hello URL-címek:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- Ha IP-címeken alapuló tűzfalszabályok szabály, győződjön meg arról, kommunikációs tooAzure lehetővé teszik.
- Hello engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443) port hello.
- Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).

Site Recovery üzembe helyezése során tooreplicate tooa Hyper-V hely kívánt virtuális gépeket tartalmazó Hyper-V-gazdagépek hozzáadása. hello Site Recovery Provider és Recovery Services Agent ügynököt minden gazdagépen van telepítve. hello Hyper-V site Recovery Services-tároló hello regisztrálva van.

## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[7. lépés: a tároló létrehozása](hyper-v-site-walkthrough-create-vault.md)

