---
title: "aaaStorSimple Snapshot Manager biztonsági mentési feladatok |} Microsoft Docs"
description: "Útmutatás a toouse StorSimple Snapshot Manager MMC beépülő modul tooview hello és ütemezett, folyamatban levő és befejezett biztonsági mentési feladatok kezelése."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: bf4dcff6-c819-4766-b9d9-9922831cb200
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 3dba0a2aa527d17d67130f537bcdce5722b05a76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-tooview-and-manage-backup-jobs"></a>StorSimple Snapshot Manager tooview használja, és a biztonsági mentési feladatok kezelése

## <a name="overview"></a>Áttekintés
Hello **feladatok** hello csomópontja **hatókör** ablaktábla megjeleníti azokat a hello **ütemezett**, **utolsó 24 óra**, és **futtató**biztonsági mentési feladatokat, interaktív vagy a beállított házirend kezdeményezett. 

Ez az oktatóanyag azt ismerteti, hogyan használhatja a hello **feladatok** ütemezett, a legutóbbi és a jelenleg futó biztonsági mentési feladatok csomópont toodisplay információt. (feladatok és a megfelelő információkkal hello listája jelenik meg hello **eredmények** ablaktáblán.) Továbbá kattintson a jobb gombbal a listában szereplő feladatok, és tekintse meg a helyi menü, amely felsorolja az elérhető műveletek.

## <a name="view-scheduled-jobs"></a>Ütemezett feladatok megtekintése
A következő eljárás tooview ütemezett biztonsági mentési feladatok hello használata.

#### <a name="tooview-scheduled-jobs"></a>tooview ütemezett feladatok
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager. 
2. A hello **hatókör** ablaktáblában bontsa ki a hello **feladatok** csomópontot, majd kattintson **ütemezett**. hello következő jelennek meg adatok a hello **eredmények** panelen:
   
   * **Név** – hello hello ütemezett pillanatkép neve
   * **Ezután futtassa** – hello dátum és a következő ütemezett pillanatképre hello időpontja
   * **Legutóbbi futtatás** – hello dátum meghatározott időpontjakor hello legutóbbi ütemezett pillanatkép
     
     > [!NOTE]
     > Egyszeri csak pillanatképeket hello **következő futásra** és **utolsó futtatása** hello azonos lesz.
     
     ![Ütemezett biztonsági mentési feladatok](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. További műveletek tooperform feladatról, kattintson a jobb gombbal a hello feladat nevére a hello **eredmények** ablaktábla és hello menüpontok közül választhat.

## <a name="view-recent-jobs"></a>Legutóbbi feladatok megtekintése
Használja a következő eljárás tooview biztonsági mentés hello és visszaállítási feladatok végrehajtott hello az elmúlt 24 órában.

#### <a name="tooview-recent-jobs"></a>tooview legutóbbi feladatokra
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblában bontsa ki a hello **feladatok** csomópontot, majd kattintson **utolsó 24 óra**. Hello **eredmények** ablaktábla megjeleníti azokat hello tartozó biztonsági mentési feladatok utolsó 24 órában (tooa legfeljebb 64 feladatok). hello következő jelennek meg adatok a hello **eredmények** ablaktáblán, attól függően, hogy hello **nézet** lehetőségek:
   
   * **Név** – hello hello ütemezett pillanatkép neve.
   * **Lépések** – hello dátum és idő, amikor hello pillanatkép kezdete.
   * **Leállítva** – hello dátum és a hello pillanatkép befejeződött és le lett állítva.
   * **Eltelt** – hello időn közötti hello **elindítva** és **leállítva** alkalommal.
   * **Állapot** – hello nemrég befejeződött hello feladat állapota. **Sikeres** azt jelzi, hogy hello biztonsági mentése sikeresen létrejött. **Nem sikerült** azt jelzi, hogy hello feladat futtatása sikertelen volt.
   * **Információ** – hello hello sikertelenségének okát.
   * **Memória (MB) feldolgozott** – hello adatmennyiség (MB-ban) feldolgozott hello kötet csoportból. 
     
     ![Feladatok a hello futott az elmúlt 24 órában](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. További műveletek tooperform feladatról, kattintson a jobb gombbal a hello feladat nevére a hello **eredmények** ablaktábla és hello menüpontok közül választhat.
   
    ![Egy feladat törlése](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a>Jelenleg futó feladatok megtekintése
A következő eljárás tooview jelenleg futó feladatok hello használata.

#### <a name="tooview-currently-running-jobs"></a>jelenleg futó feladatok tooview
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblában bontsa ki a hello **feladatok** csomópontot, majd kattintson **futtató**. Attól függően, hogy hello **nézet** beállításokat ad meg, a következő információk hello hello megjelenik **eredmények** panelen:
   
   * **Név** – hello hello ütemezett pillanatkép neve.
   * **Lépések** – hello dátum és idő, amikor hello pillanatkép kezdete.
   * **Ellenőrzőpont** – hello hello biztonsági mentés az aktuális művelet.
   * **Állapot** – hello feldolgozottsági százalékát.
   * **Eltelt** – hello mennyisége hello biztonsági mentés óta eltelt időt. 
   * **Átlagos átviteli sebessége (MB)** – a teljes ideje (MB) a feldolgozott adatok toothat mérete bájtban megadva aránya.
   * **Memória (MB) feldolgozott** – az adatok feldolgozása (MB) bájtok teljes száma.
   * **(MB) írt bájtok** – az adatok írása (MB-ban) bájtok teljes száma. Hello adatok, valamint a hello metaadatokat tartalmaz, és ezért a általában nagyobb, mint a bájtok feldolgozott hello.
     
     ![Jelenleg futó feladatok](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. További műveletek tooperform feladatról, kattintson a jobb gombbal a hello feladat nevére a hello **eredmények** ablaktábla és hello menüpontok közül választhat.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[használja a StorSimple Snapshot Manager tooadminister a StorSimple megoldásban](storsimple-snapshot-manager-admin.md).
* Ismerje meg, hogyan túl[StorSimple Snapshot Manager toomanage hello biztonságimásolat-katalógus használatáról az](storsimple-snapshot-manager-manage-backup-catalog.md).

