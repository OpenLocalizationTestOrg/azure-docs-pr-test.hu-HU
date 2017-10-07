---
title: "aaaStorSimple Snapshot Manager kötet csoportok |} Microsoft Docs"
description: "Útmutatás a toouse StorSimple Snapshot Manager MMC beépülő modul toocreate hello és kötet csoportok kezelése."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 7a232414-6a28-4b81-bd7b-cf61e28b33d7
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: f7830b62761db7aa5139456b555b6168fb6a85de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toocreate-and-manage-volume-groups"></a>StorSimple Snapshot Manager toocreate használja, és a kötet csoportok kezelése
## <a name="overview"></a>Áttekintés
Használhatja a hello **kötet csoportok** hello csomópont **hatókör** mentéseket ablaktábla tooassign kötetek toovolume csoportok, egy kötet csoport adatainak megtekintése és a kötet csoportok szerkesztése.

Kötet csoportok azonban a kapcsolódó kötetek használt tooensure, hogy a biztonsági mentések alkalmazáskonzisztens. További információkért lásd: [kötetek és a kötet csoportok](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) és [integráció a Windows a kötet árnyékmásolata szolgáltatás](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

> [!IMPORTANT]
> * Egy kötet csoportban található összes kötetet egy felhőszolgáltató kell származnia.
> * Kötet csoportok konfigurálásakor nem keverhetők a fürt megosztott kötetei (CSV-k) és a CSV a hello kötet ugyanabban a csoportban. StorSimple Snapshot Manager nem támogatja a CSV-k kombinációját, és a nem megosztott hello azonos pillanatkép.

![Kötet csoportok csomópont](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**1. ábra: A StorSimple Snapshot Manager kötet csoportok csomópont** 

Ez az oktatóanyag azt ismerteti, hogyan használhatja a StorSimple Snapshot Manager:

* A kötet csoportok adatainak megtekintése
* Kötet csoport létrehozása
* Készítsen biztonsági másolatot egy kötet csoport
* Kötet csoport szerkesztése
* Kötet csoport törlése

Ezek a műveletek mindegyikét is elérhetők a hello **műveletek** ablaktáblán.

## <a name="view-volume-groups"></a>Kötet csoportok megtekintése
Ha hello **kötet csoportok** csomópont, hello **eredmények** ablaktábla megjeleníti azokat a következő információk minden kötet csoportról hello, attól függően, hogy hello oszlopkiválasztásokat elvégezte. (hello oszlopai hello **eredmények** ablaktáblán is konfigurálható. Kattintson a jobb gombbal hello **kötetek** csomópontban jelölje ki **nézet**, majd válassza ki **oszlopok hozzáadása/eltávolítása**.)

| Eredmények oszlop | Leírás |
|:--- |:--- |
| Név |Hello **neve** oszlop hello kötet csoport hello nevét tartalmazza. |
| Alkalmazás |Hello **alkalmazások** az oszlopban látható a jelenleg telepített VSS-író hello száma és a Windows-gazdagépen futó hello. |
| Kiválasztva |Hello **kijelölt** az oszlopban látható található kötetek számát hello hello kötet csoportban. A nulla (0) azt jelzi, hogy egyetlen alkalmazás hello kötetek hello kötet csoport társítva. |
| Importált |Hello **importált** oszlop importált kötetek hello számát mutatja. Ha értéke túl**igaz**, ebben az oszlopban azt jelzi, hogy a kötet csoport importált hello Azure-portálon, és nem jött létre a StorSimple Snapshot Manager. |

> [!NOTE]
> StorSimple Snapshot Manager kötet csoport is megjelenik a hello **biztonsági mentési házirendek** hello Azure-portálon lapján.
> 
> 

## <a name="create-a-volume-group"></a>Kötet csoport létrehozása
A következő eljárás toocreate egy kötet csoport hello használata.

#### <a name="toocreate-a-volume-group"></a>a kötet csoport toocreate
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblán kattintson a jobb gombbal **kötet csoportok**, és kattintson a **kötet csoport létrehozása**.
   
    ![Kötet csoport létrehozása](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
   
    Hello **hozzon létre egy kötet csoportot** párbeszédpanel jelenik meg.
   
    ![Hozzon létre egy kötet csoport párbeszédpanel](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png)
3. Adja meg a következő információ hello:
   
   1. A hello **neve** mezőbe írjon be egy egyedi nevet hello új kötet csoport.
   2. A hello **alkalmazások** mezőben, válassza ki, hogy fel szeretné venni toohello kötet csoport hello kötetek társított alkalmazások.
      
       Hello **alkalmazások** mezőben listák csak ezeket az alkalmazásokat, amelyek a StorSimple-köteteket használ, és rendelkezik a VSS-írók engedélyezve van, a számukra. VSS-író szolgáltatás engedélyezve van, csak ha összes hello kötetek tisztában-e, hogy hello író vannak a StorSimple-köteteket. Ha hello alkalmazások mező üres, amely Azure StorSimple-köteteket használ, és támogatott van-e VSS-író alkalmazás sem lesz telepítve. (Jelenleg Azure StorSimple támogatja a Microsoft Exchange, SQL Server.) További információ a VSS-író: [integráció a Windows a kötet árnyékmásolata szolgáltatás](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).
      
       Ha kiválaszt egy alkalmazást, akkor hozzá társított összes kötet automatikusan ki van jelölve. Ezzel szemben, ha egy adott alkalmazáshoz társított kötetek, hello alkalmazás automatikusan ki van jelölve a hello **alkalmazások** mezőbe. 
   3. A hello **kötetek** jelölje ki a StorSimple kötetek tooadd toohello kötet csoport. 
      
      * Megadhat egy vagy több partíciókkal rendelkező köteteket. (Több partíció kötetek lehetnek dinamikus lemezek vagy alaplemezek több partícióval rendelkező.) A kötet, amely több partíciót tartalmaznak a rendszer egyetlen egységként kezeli. Ezért ha többi partíció hozzáadása csak az egyik hello partíciók tooa kötet csoport összes hello vannak automatikusan hozzáadott toothat kötet csoportjának hello, azonos idő. Egy több partíció kötet tooa kötet csoport hozzáadása után hello több partíció kötet továbbra is egyetlen egységként kezelt toobe.
      * Bármely kötetek toothem nem hozzárendelésével üres kötet csoportokat is létrehozhat. 
      * Ne keverje a fürt megosztott kötetei (CSV-k) és a CSV a hello kötet ugyanabban a csoportban. StorSimple Snapshot Manager nem támogatja a megosztott fürtkötetek kombinációját, és nem CSV-köteteket a hello azonos pillanatkép.
4. Kattintson a **OK** toosave hello kötet csoport.

## <a name="back-up-a-volume-group"></a>Készítsen biztonsági másolatot egy kötet csoport
A következő eljárás tooback kötet csoport hello használata.

#### <a name="tooback-up-a-volume-group"></a>kötet csoport tooback
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblában bontsa ki a hello **kötet csoportok** csomópontot, kattintson a jobb gombbal egy kötet felügyeleticsoport-nevet, és kattintson **érvénybe biztonsági mentés**.
   
    ![Készítsen biztonsági másolatot kötet csoport azonnal](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)
3. A hello **érvénybe biztonsági mentés** párbeszédpanelen jelölje ki **helyi pillanatfelvétel** vagy **felhőalapú pillanatfelvétel**, és kattintson a **létrehozása**.
   
    ![Igénybe vehet a biztonsági mentési párbeszédpanel](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png)
4. amely a biztonsági mentési hello tooconfirm fut, bontsa ki a hello **feladatok** csomópontra, majd **futtató**. hello biztonsági mentést kell szerepelnie.
5. befejeződött tooview hello pillanatkép, bontsa ki a hello **biztonságimásolat-katalógus** csomópontot, bontsa ki hello kötet csoport nevét, majd **helyi pillanatfelvétel** vagy **felhőalapú pillanatfelvétel**. hello biztonsági mentés megjelenik, ha sikeresen befejeződött.

## <a name="edit-a-volume-group"></a>Kötet csoport szerkesztése
A következő eljárás tooedit egy kötet csoport hello használata.

#### <a name="tooedit-a-volume-group"></a>a kötet csoport tooedit
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblában bontsa ki a hello **kötet csoportok** csomópontot, kattintson a jobb gombbal egy kötet felügyeleticsoport-nevet, és kattintson **szerkesztése**.
3. hello ** hozzon létre egy kötet csoportot ** párbeszédpanel jelenik meg. Módosíthatja a hello **neve**, **alkalmazások**, és **kötetek** bejegyzéseket.
4. Kattintson a **OK** toosave a módosításokat.

## <a name="delete-a-volume-group"></a>Kötet csoport törlése
A következő eljárás toodelete egy kötet csoport hello használata. 

> [!WARNING]
> Ez a hello kötet csoporthoz társított összes hello biztonsági mentés is törli.
> 
> 

#### <a name="toodelete-a-volume-group"></a>a kötet csoport toodelete
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblában bontsa ki a hello **kötet csoportok** csomópontot, kattintson a jobb gombbal egy kötet felügyeleticsoport-nevet, és kattintson **törlése**.
3. Hello **kötet csoport törlése** párbeszédpanel jelenik meg. Típus **megerősítése** hello szövegmezőbe, és kattintson a **OK**.
   
    törölt hello kötet csoport eltűnik hello hello listájából **eredmények** ablaktábla és az adott kötet csoporthoz rendelt összes biztonsági mentés hello biztonságimásolat-katalógus törlődnek.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[használja a StorSimple Snapshot Manager tooadminister a StorSimple megoldásban](storsimple-snapshot-manager-admin.md).
* Ismerje meg, hogyan túl[StorSimple Snapshot Manager toocreate használatát és kezelését a biztonsági mentési házirendek](storsimple-snapshot-manager-manage-backup-policies.md).

