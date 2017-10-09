---
title: "egy replikációs házirendnek a fizikai kiszolgáló replikációs tooAzure az Azure Site Recovery aaaSet |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket végre kell tooset fel egy replikációs házirendet, ha replikálása a helyszíni fizikai kiszolgálók tooAzure tárolási hello Azure Site Recovery szolgáltatással"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d7874bd8-6626-4668-9ec9-dbd2d26f8f81
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: d6bdeeffc24ffc8eaba24311f7c76452edb65648
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-a-replication-policy-for-physical-server-replication-tooazure"></a>8. lépés: A fizikai kiszolgáló replikációs tooAzure replikációs házirend beállítása


Ez a cikk ismerteti, hogyan tooset fel egy replikációs házirendet, ha hello használata windowsos/Linuxos fizikai kiszolgálók tooAzure replikál [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.


Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="configure-a-policy"></a>Házirend konfigurálása

1. Kattintson a **Site Recovery-infrastruktúra** > **replikációs házirendek** > **+ replikációs házirend**.
2. A **replikációs házirend létrehozása**, megadhatja a házirend nevét.
3. A **helyreállítási Időkorlát küszöbértéke**, adja meg a hello RPO korlátot. Ez az érték azt jelzi, hogy milyen gyakran adatok helyreállítási pontot hoz létre. Riasztás akkor jön létre, ha a folyamatos replikálásra túllépi ezt a korlátozást.
4. A **helyreállításipont-megőrzést**, adja meg (órában) mennyi ideig áll hello adatmegőrzési időtartam az egyes helyreállítási pontok. A replikált virtuális gépek lehet helyreállítani tooany pont ablakban. Másolatot óra megőrzési gépek esetén támogatott too24 replikált toopremium tárolás és a standard szintű tárolást 72 órát.
5. A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg, milyen gyakran (percben) alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontokat hoz létre. Kattintson a **OK** toocreate hello házirend.

    ![Replikációs szabályzat](./media/physical-walkthrough-replication/gs-replication2.png)
8. Ha egy új házirendet hoz létre, automatikusan rendelkezik hello konfigurációs kiszolgáló társítva. Alapértelmezés szerint a megfelelő házirend automatikusan létrejön a feladat-visszavételre. Például ha hello replikációs házirend nem **rep-házirend** akkor hello feladatátvételi házirendhez lesz **rep-házirend-feladat-visszavétel**. Ez a házirend nem használ, amíg nem indítja el a feladat-visszavétel az Azure-ból.

## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[9. lépés: hello mobilitási szolgáltatás telepítése](physical-walkthrough-install-mobility.md)
