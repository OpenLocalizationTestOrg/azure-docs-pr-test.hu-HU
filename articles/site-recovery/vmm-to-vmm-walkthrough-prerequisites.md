---
title: "Hyper-V replikáció tooa VMM másodlagos hely az Azure Site Recovery aaaReview hello előfeltételei |} Microsoft Docs"
description: "A Hyper-V virtuális gépek tooa VMM másodlagos hely az Azure Site Recovery replikálására hello Előfeltételek ismerteti."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 21ff0545-8be5-4495-9804-78ab6e24add6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 1bd945fdda36c3cce5d159209abbd3c98a7e3682
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-and-limitations-for-hyper-v-vm-replication-tooa-secondary-vmm-site"></a>2. lépés: Tekintse át a Hyper-V virtuális gép replikációs tooa VMM másodlagos hely vonatkozó hello Előfeltételek és korlátozások


Miután már tekintse át a hello [kialakítandó architektúra](vmm-to-vmm-walkthrough-architecture.md), olvassa el a cikk toomake hello telepítés előfeltételeit, így megértheti, amikor replikálása a helyszíni Hyper-V virtuális gépek (VM) kezelése a System Center virtuális A felhők Machine Manager (VMM) tooa másodlagos webhelyen segítségével [Azure Site Recovery](site-recovery-overview.md) a hello Azure-portálon.

A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites-and-limitations"></a>Előfeltételek és korlátozások

**Követelmény** | **Részletek**
--- | ---
**Azure** | A [Microsoft Azure](http://azure.microsoft.com/) előfizetés.<br/><br/> Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.<br/><br/> [További információk](https://azure.microsoft.com/pricing/details/site-recovery/) a Site Recovery díjszabásáról.<br/><br/> Hello támogatott régiók ellenőrizze a hely helyreállítását követően a cikknek a földrajzi elérhetőséggel a [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/).
**VMM-kiszolgálókon** | Javasoljuk, hogy a két VMM-kiszolgáló, egy hello elsődleges hely és másodlagos hello egyet-egyet.<br/><br/> Egyetlen felhők közötti replikálása VMM-kiszolgálón támogatott.<br/><br/> VMM-kiszolgálókon legalább futnia kell a System Center 2012 SP1 hello legújabb frissítéseit.<br/><br/> VMM-kiszolgáló internetes hozzáférésre van szükségük.
**VMM-felhő** | Minden VMM-kiszolgálót rendelkeznie kell egy vagy több felhőt, és az összes felhő rendelkeznie kell beállított hello Hyper-V kapacitás profil. <br/><br/>Felhő legalább egy VMM-gazdagépcsoportot kell tartalmaznia.<br/><br/> Csak egy VMM-kiszolgálóval rendelkezik, hogy szükséges-e legalább két felhők, tooact elsődleges és másodlagos.
**Hyper-V** | Hyper-V kiszolgálók legalább futnia kell hello Hyper-V szerepkör és a Windows Server 2012 és a legújabb frissítések telepítve rendelkezik hello.<br/><br/> Minden Hyper-V kiszolgáló tartalmazzon legalább egy virtuális gépet.<br/><br/>  Hyper-V gazdakiszolgálók gazdagépcsoportok hello elsődleges és másodlagos VMM-felhőkben kell elhelyezkedniük.<br/><br/> Ha a Hyper-V fürt, a Windows Server 2012 R2 rendszeren futtatja, telepítse [2961977 frissítése](https://support.microsoft.com/kb/2961977)<br/><br/> Ha a Hyper-V fürt, a Windows Server 2012 rendszerben futtatja, fürtszervező nem hozza létre automatikusan Ha egy statikus IP cím alapú fürtöt. Hello fürtszervező kézi konfigurálását. [További információk](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).<br/><br/> Hyper-V kiszolgálók internet-hozzáférésre van szükségük.




## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[3. lépés: hálózati terv](vmm-to-vmm-walkthrough-network.md).
