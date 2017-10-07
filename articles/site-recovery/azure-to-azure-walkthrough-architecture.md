---
title: "Azure virtuális gépek Azure-régiók közötti replikáció aaaReview hello architektúra |} Microsoft Docs"
description: "Ez a cikk áttekintése összetevők és architektúra használható, ha az Azure virtuális gépek replikálása Azure-régiók hello Azure Site Recovery szolgáltatás között."
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
ms.date: 07/12/2017
ms.author: raynew
ms.openlocfilehash: 4caab4e7a764040f317201d1345c40c73f836d81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-azure-vm-replication-between-azure-regions"></a>1. lépés:, Tekintse át az Azure-régiók közötti Azure virtuális gép replikációs hello architektúrája


Hello áttekintése után [áttekintése lépéseket](azure-to-azure-walkthrough-overview.md) ehhez a központi telepítéshez, olvassa el a cikk toounderstand hello összetevők és a folyamatok használható, ha a replikálása, és végezze el az Azure-régió tooanother egy, a Azure virtuális gépek (VM) használatával [Azure Site Recovery](site-recovery-overview.md).

- Hello cikk befejezése után kell egy Azure virtuális gép replikációs tooanother régió működése egyértelműen érthető.
- Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
>A Site Recovery szolgáltatás hello Azure Virtuálisgép-replikációt jelenleg előzetes verzió.



## <a name="architectural-components"></a>Az architektúra összetevői

a következő diagram hello Azure virtuális környezetben (a példában hello USA keleti régiója helye) egy adott régióban magas szintű áttekintést nyújt. Egy Azure virtuális környezetben:
- Storage-fiókok elosztva lemezek alkalmazásokat futtathat virtuális gépeken.
- virtuális gépek hello tartalmazhat egy vagy több alhálózatot a virtuális hálózaton belül.

![ügyfél-környezet](./media/azure-to-azure-walkthrough-architecture/source-environment.png)

## <a name="replication-process"></a>Replikációs folyamat

### <a name="step-1"></a>1. lépés

Ha engedélyezi az Azure virtuális gép replikálása az Azure-portálon hello, hello erőforrások látható hello a következő ábra és táblázat automatikusan létrejönnek hello cél régióban. Alapértelmezés szerint erőforrások forrásbeállítások régió alapján hozzák létre. Hello tárolóbeállítások szükség szerint testre. [További információk](site-recovery-replicate-azure-to-azure.md).

![Engedélyezze a replikálási folyamat, 1. lépés](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-1.png)

**Erőforrás** | **Részletek**
--- | ---
**Cél-erőforráscsoport** | hello erőforrás csoport toowhich replikált virtuális gépek a feladatátvételt követően tartozik.
**Virtuális hálózati cél** | hello virtuális hálózatot, amelyben a replikált virtuális gépek a feladatátvétel után. A hálózatleképezés jön létre a forrás és cél virtuális hálózatok között, és ez fordítva is igaz.
**Gyorsítótár-storage-fiókok** | A forrás virtuális gépeken változásai replikálódnak a céloldali tárfiók toohello, mielőtt ezeket nyomon követheti és toohello gyorsítótár tárfiók küldi hello célhelyet. Ez biztosítja, hogy a futó virtuális gép hello éles alkalmazásokra gyakorolt minimális hatás mellett.
**Cél storage-fiókok**  | Storage-fiók hello cél toowhich hello helyadatok replikálódnak.
**Cél rendelkezésre állási csoportok**  | Rendelkezésre állási készletek a mely hello replikált virtuális gépek a feladatátvétel után.

### <a name="step-2"></a>2. lépés

Replikálás engedélyezve van, mert hello Site Recovery bővítmény mobilitási szolgáltatás automatikusan települ azon hello virtuális gép. hello következő történik:

1. hello VM regisztrálva van a Site Recovery szolgáltatással.

2. Virtuális gép hello folyamatos replikálásra van konfigurálva. Adatok írása hello méretű lemezek vannak folyamatosan toohello gyorsítótár tárfiók hello forráshely át.

   ![Engedélyezze a replikálási folyamat, 2. lépés](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-2.png)

  
  Vegye figyelembe, hogy a Site Recovery soha nem kell kapcsolatot toohello VM bejövő. Csak kimenő kapcsolat tooSite helyreállítási szolgáltatás URL-címek vagy IP-címek, Office 365 portál URL-címek vagy IP-címek és gyorsítótár tárolási fiók IP-címek van szükség. 

## <a name="continuous-replication-process"></a>Folyamatos replikálási folyamat

Után folyamatos replikálás működik-e, a lemez írási műveleteket azonnal lépnek át toohello gyorsítótár tárfiók. A Site Recovery hello adatokat dolgozza fel, és elküldi azt toohello cél tárfiók. Hello adatok feldolgozása után helyreállítási pontok jönnek létre a céloldali tárfiók hello néhány percenként.

## <a name="failover-process"></a>Feladatátvételi folyamat

Kezdeményezzen feladatátvételt, virtuális gépek jönnek létre hello célerőforrás-csoport, a cél virtuális hálózat, a cél alhálózathoz hello és hello rendelkezésre állási csoportot célozza. A feladatátvétel során bármely helyreállítási pontot is használhat.

![Feladatátvételi folyamat](./media/azure-to-azure-walkthrough-architecture/failover.png)

## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[2. lépés: Ellenőrizze az Előfeltételek és korlátozások](azure-to-azure-walkthrough-prerequisites.md)
