---
title: "aaaStorSimple Snapshot Manager biztonsági mentési házirendek |} Microsoft Docs"
description: "Útmutatás a toouse StorSimple Snapshot Manager MMC beépülő modul toocreate hello és hello biztonsági mentési házirend ütemezett biztonsági mentések szabályozó kezeléséhez."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 04415d0b-42f0-4737-8afa-257fb2dbe5d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: c2ae75a8d0568090add6018da18de73eb56e6590
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toocreate-and-manage-backup-policies"></a>StorSimple Snapshot Manager toocreate használatát és kezelését a biztonsági mentési házirendek
## <a name="overview"></a>Áttekintés
A biztonsági mentési házirend hoz létre egy ütemezést az adatok biztonsági mentése kötet helyileg vagy hello felhőben. A biztonsági mentési házirend létrehozásakor adja meg egy megőrzési házirend. (Legfeljebb 64 pillanatképek őrizheti). További információ a biztonsági mentési házirendek: [biztonsági mentési típusok](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) a [StorSimple 8000 series: hibrid felhőalapú megoldás](storsimple-overview.md).

Ez az oktatóanyag azt ismerteti, hogyan:

* A biztonsági mentési házirend létrehozása
* A biztonsági mentési házirend szerkesztése
* A biztonsági mentési házirend törlése

## <a name="create-a-backup-policy"></a>A biztonsági mentési házirend létrehozása
A következő eljárás toocreate egy új biztonsági mentési házirend hello használata.

#### <a name="toocreate-a-backup-policy"></a>a biztonsági mentési házirend toocreate
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblán kattintson a jobb gombbal **biztonsági mentési házirendek**, és kattintson a **biztonsági mentési házirend létrehozása**.

    ![A biztonsági mentési házirend létrehozása](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    Hello **házirend létrehozása** párbeszédpanel jelenik meg.

    ![Hozzon létre egy házirend – Általános lap](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. A hello **általános** lap, a következő információ teljes hello:

   1. A hello **neve** szövegmezőbe írja be a hello házirend nevét.
   2. A hello **kötet csoport** szövegmezőben, hello kötet csoport hello házirendhez társított hello nevét.
   3. Válassza ki vagy **helyi pillanatfelvétel** vagy **felhőalapú pillanatfelvétel**.
   4. Válassza ki a pillanatképek tooretain hello számát. Ha **összes**, 64 pillanatfelvételek megmaradnak (hello maximális).
4. Kattintson a hello **ütemezés** fülre.

    ![Hozzon létre egy házirend - ütemezés lapon](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. A hello **ütemezés** lap, a következő információ teljes hello:

   1. Kattintson a hello **engedélyezése** jelölőnégyzetet tooschedule hello következő biztonsági mentés.
   2. A **beállítások**, jelölje be **egyszer**, **napi**, **heti**, vagy **havi**.
   3. A hello **Start** szövegmezőben kattintson hello naptár ikonra, és válassza ki a kezdési dátum.
   4. A **speciális beállítások**, beállíthatja a választható ismétlődő ütemezés és a befejező dátumát.
   5. Kattintson az **OK** gombra.

Miután létrehozta a biztonsági mentési házirend, a következő információk hello megjelenik hello **eredmények** panelen:

* **Név** – hello biztonsági mentési házirend nevét.
* **Típus** – helyi pillanatfelvétel és felhőbeli pillanatfelvétel.
* **Kötet csoport** – hello hello házirendhez társított kötet csoport.
* **Megőrzési** – hello pillanatképek számát maradnak; hello maximális 64.
* **Létrehozott** – Ez a házirend létrehozásának hello dátuma.
* **Engedélyezett** – hogy hello házirend van érvényben: **igaz** azt mutatja, hogy érvényes; **Hamis** azt mutatja, hogy nem érvényes.

## <a name="edit-a-backup-policy"></a>A biztonsági mentési házirend szerkesztése
A következő eljárás tooedit egy meglévő biztonsági mentési házirend hello használata.

#### <a name="tooedit-a-backup-policy"></a>a biztonsági mentési házirend tooedit
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblán kattintson a hello **biztonsági mentési házirendek** csomópont. Minden hello biztonsági mentési házirendek jelennek-e hello **eredmények** ablaktáblán.
3. Kattintson a jobb gombbal a kívánt tooedit, és kattintson hello házirend **szerkesztése**.

    ![A biztonsági mentési házirend szerkesztése](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. Ha hello **házirend létrehozása** ablak megjelenik, írja be a módosításokat, és kattintson **OK**.

## <a name="delete-a-backup-policy"></a>A biztonsági mentési házirend törlése
A következő eljárás toodelete a biztonsági mentési házirend hello használata.

#### <a name="toodelete-a-backup-policy"></a>a biztonsági mentési házirend toodelete
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblán kattintson a hello **biztonsági mentési házirendek** csomópont. Minden hello biztonsági mentési házirendek jelennek-e hello **eredmények** ablaktáblán.
3. Kattintson a jobb gombbal a biztonsági mentési házirend hello toodelete szeretne, és kattintson **törlése**.
4. Hello jóváhagyást kérő üzenet megjelenésekor kattintson **Igen**.

    ![A biztonsági mentési házirend megerősítés törlése](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[használja a StorSimple Snapshot Manager tooadminister a StorSimple megoldásban](storsimple-snapshot-manager-admin.md).
* Ismerje meg, hogyan túl[StorSimple Snapshot Manager tooview használatát és kezelését a biztonsági mentési feladatok](storsimple-snapshot-manager-manage-backup-jobs.md).
