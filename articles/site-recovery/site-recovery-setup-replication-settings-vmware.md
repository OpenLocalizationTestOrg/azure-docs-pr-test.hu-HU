---
title: "az Azure Site Recovery replikációs beállítások aaaSet |} Microsoft Docs"
description: "Ismerteti, hogyan toodeploy Site Recovery tooorchestrate replikációs, feladatátvételével és helyreállításával Hyper-V virtuális gépek a VMM-felhők tooAzure."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: rayne-wiselman
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: 618e92e42411732a2a1bb75c5e5ea8a433cd7d58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-replication-policy-for-vmware-tooazure"></a>A VMware tooAzure replikációs házirend kezelése


## <a name="create-a-replication-policy"></a>Replikációs házirend létrehozása

1. Válassza a **Kezelés** > **Site Recovery-infrastruktúra** lehetőséget.
2. Válassza a **Replikációs házirendek** lehetőséget a **VMware- és fizikai gépek számára** területen.
3. Válassza a **+Replikációs házirend** lehetőséget.

    ![Replikációs házirend létrehozása](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. Adja meg a hello házirend nevét.

5. A **helyreállítási Időkorlát küszöbértéke**, adja meg a hello RPO korlátot. A rendszer riasztásokat küld, ha a folyamatos replikáció meghaladja ezt a korlátot.
6. A **helyreállításipont-megőrzést**, adja meg (órában) hello hello megőrzési időszak az egyes helyreállítási pontok időtartama. Védett gépek lehet helyreállítani egy adatmegőrzési időtartam tooany pontot.

    > [!NOTE]
    > Adatmegőrzési too24 órában gépek replikált toopremium tárolási támogatott. Adatmegőrzési too72 órában gépek replikált toostandard tárolási támogatott.

    > [!NOTE]
    > A rendszer automatikusan hoz létre replikációs házirendet a feladat-visszavételhez.

7. Az **Alkalmazáskonzisztens pillanatkép gyakorisága** beállítás azt határozza meg, hogy milyen gyakran hozzon létre a rendszer alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontokat (a beállítás értéke percekben adható meg).

8. Kattintson az **OK** gombra. hello házirend létre kell hozni too60 30 másodperc.

![Replikációs házirend létrehozása](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-a-configuration-server-with-a-replication-policy"></a>Konfigurációs kiszolgáló társítása egy replikációs házirenddel
1. Válassza ki a hello replikációs házirend toowhich tooassociate hello konfigurációs kiszolgálót.
2. Kattintson a **Társítás** gombra.
![Konfigurációs kiszolgáló társítása](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)

3. Válassza ki a hello konfigurációs kiszolgáló hello azon kiszolgálók listájáról.
4. Kattintson az **OK** gombra. hello konfigurációs kiszolgáló egy tootwo percben kell rendelhetők hozzá.

![A konfigurációs kiszolgáló társítása](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-a-replication-policy"></a>Replikációs házirend szerkesztése
1. Válassza ki a kívánt tooedit replikációs beállítások hello replikációs házirend.
![Replikációs házirend szerkesztése](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)

2. Kattintson a **Beállítások szerkesztése** lehetőségre.
![Replikációs házirend beállításainak szerkesztése](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)

3. Módosítsa a igények alapján hello beállításait.
4. Kattintson a **Save** (Mentés) gombra. hello házirend mentésére két toofive percben, attól függően, hogy hány virtuális gépeket használ, hogy a replikációs házirend.

![Replikációs házirend mentése](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-a-configuration-server-from-a-replication-policy"></a>A konfigurációs kiszolgáló és a replikációs házirend társításának megszüntetése
1. Válassza ki a hello replikációs házirend toowhich tooassociate hello konfigurációs kiszolgálót.
2. Kattintson a **Társítás megszüntetése** gombra.
3. Válassza ki a hello konfigurációs kiszolgáló hello azon kiszolgálók listájáról.
4. Kattintson az **OK** gombra. hello konfigurációs kiszolgáló egy tootwo percben kell különíthetők el.

    > [!NOTE]
    > A konfigurációs kiszolgáló nem lehet leválasztani, ha legalább egy replikált elem hello házirenddel. Ellenőrizze, hogy nincsenek hello házirend használata előtt hello konfigurációs kiszolgáló megszünteti replikált elemek.

## <a name="delete-a-replication-policy"></a>Replikációs házirend törlése

1. Válassza ki a hello replikációs házirendet, amelyet az toodelete.
2. Kattintson a **Törlés** gombra. hello házirend törölni kell a too60 30 másodperc.

    > [!NOTE]
    > Replikációs házirend nem törölhető, ha legalább egy konfigurációs kiszolgálóhoz kapcsolódó tooit van. Ellenőrizze, hogy nincsenek hello házirenddel replikált elemek, és törölje az összes hello kapcsolódó konfigurációs kiszolgálók hello házirend törlése előtt.
