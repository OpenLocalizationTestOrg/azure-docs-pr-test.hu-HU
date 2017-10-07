---
title: "aaaMicrosoft Azure StorSimple virtuális tömb biztonsági mentési oktatóanyag |} Microsoft Docs"
description: "Ismerteti, hogyan osztja meg a StorSimple virtuális tömb mentése tooback és köteteket."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: e3cdcd9e-33b1-424d-82aa-b369d934067e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7a015fd594f8f56c48fab149a2736be9dec2c24b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a>A StorSimple virtuális tömb biztonsági másolatot a megosztások vagy kötetek

## <a name="overview"></a>Áttekintés

a StorSimple virtuális tömb hello egy hibrid felhő helyszíni virtuális tárolóeszköz, konfigurálhat egy fájl vagy iSCSI-kiszolgáló. hello virtuális tömb lehetővé teszi, hogy hello felhasználói toocreate ütemezett és manuális biztonsági mentés minden hello megosztások vagy kötetek hello eszközön. Amikor egy fájl-kiszolgálóként konfigurált lehetővé teszi elemszintű helyreállítás. Ez az oktatóanyag ismerteti, hogyan toocreate ütemezett és manuális biztonsági mentések, és végezze el a virtuális tömb törölt fájlok elemszintű helyreállítás toorestore.

Ez az oktatóanyag toohello StorSimple virtuális tömbök csak vonatkozik. 8000 Series információkért nyissa meg túl[8000 sorozat eszköz biztonsági mentés létrehozása](storsimple-manage-backup-policies-u2.md)

## <a name="back-up-shares-and-volumes"></a>Megosztások és a kötetek biztonsági mentése

Biztonsági mentések időpontban védelmet, javítja a helyreállíthatóságot és megosztásokat és -kötetek visszaállítása idejének minimalizálása érdekében. Biztonsági másolatot készíthet egy fájlmegosztás vagy kötet StorSimple eszközén kétféle módon: **ütemezett** vagy **manuális**. A következő szakaszok hello hello módszerek ismertet.

## <a name="change-hello-backup-start-time"></a>Hello biztonsági mentés kezdési ideje

> [!NOTE]
> Ebben a kiadásban ütemezett biztonsági mentések hozza létre, amely egy megadott időpontban naponta fut, és készít biztonsági másolatot minden hello megosztások vagy kötetek; hello eszközön alapértelmezett házirend. Ez jelenleg nem lehetséges toocreate egyéni házirendeket az ütemezett biztonsági mentések.


A StorSimple virtuális tömb tartozik egy alapértelmezett biztonsági mentési házirend, amely egy megadott időpontot (22:30) kezdődik, és biztonsági mentést készít minden hello megosztások vagy kötetek; hello eszközön naponta egyszer. Módosíthatja a hello idő mely hello biztonsági mentés elindul, de hello gyakoriságát és hello megőrzési (amely azt adja meg a biztonsági mentések tooretain hello száma) nem módosítható. Ezek a biztonsági mentések során hello teljes virtuális eszköz biztonsági mentése. Ez sikerült potenciálisan hello eszköz hello teljesítményt, és hatással lehet a hello eszközön telepített hello munkaterhelésekhez. Ezért azt javasoljuk, hogy ezek a biztonsági mentések ütemezése kevesen.

 toochange hello alapértelmezett biztonsági mentés kezdési időpontja, hajtsa végre a következő lépéseket a hello hello [Azure-portálon](https://portal.azure.com/).

#### <a name="toochange-hello-start-time-for-hello-default-backup-policy"></a>toochange hello kezdő időpont hello alapértelmezett biztonsági mentési házirend

1. Nyissa meg túl**eszközök**. a StorSimple Device Manager szolgáltatásban regisztrált eszközök hello listája jelenik meg. 
   
    ![Keresse meg a toodevices](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. Jelölje ki, majd kattintson az eszköz. Hello **beállítások** panel fog megjelenni. Nyissa meg túl**kezelése > biztonsági mentési házirendek**.
   
    ![az eszköz kiválasztásához](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. A hello **biztonsági mentési házirendek** panelen hello alapértelmezett kezdési ideje 22:30. Eszköz időzóna hello napi ütemezés hello új kezdési időpontot adhat meg.
   
    ![Keresse meg a toobackup házirendek](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. Kattintson a **Save** (Mentés) gombra.

### <a name="take-a-manual-backup"></a>Manuális biztonsági mentés készítése

Ezenkívül tooscheduled biztonsági mentések, végre eszközadatok manuális (igény) biztonsági mentés bármikor.

#### <a name="toocreate-a-manual-backup"></a>Manuális biztonsági mentés toocreate

1. Nyissa meg túl**eszközök**. Válassza ki azt az eszközt, és kattintson a jobb gombbal **...**  jobb szélén hello: hello kijelölt sor. Hello helyi menüből válassza ki a **biztonsági másolatok készítéséhez**.
   
    ![Keresse meg a tootake biztonsági mentése](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. A hello **biztonsági másolatok készítéséhez** panelen kattintson a **biztonsági másolatok készítéséhez**. Ez biztonsági másolatot készít minden hello megosztások fájlkiszolgáló hello vagy az iSCSI-kiszolgálón az összes hello kötetek. 
   
    ![biztonsági mentés indítása](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    Egy igény szerinti biztonsági mentés elindul, és láthatja, hogy egy biztonsági mentési feladat elindult.
   
    ![biztonsági mentés indítása](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    Miután hello feladat sikeresen befejeződött, értesítés jelenik meg újra. Ezután elindítja hello biztonsági mentési folyamat.
   
    ![Biztonsági mentési feladat létrehozása](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. tootrack hello előrehaladását hello biztonsági mentéseit és nézze meg hello feladat részleteit, kattintson a hello értesítés. Ezzel megnyitná túl **feladat részletei**.
   
     ![biztonsági mentési feladat részleteit](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. Hello biztonsági mentés befejezése után, nyissa meg túl**felügyeleti > biztonságimásolat-katalógus**. Látni fogja a felhő-pillanatfelvételt a összes hello megosztások (vagy kötetek;) az eszközön.
   
    ![Befejezett biztonsági mentése](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a>Meglévő biztonsági másolatok megtekintése
tooview hello létező biztonsági másolatai, hajtsa végre a következő lépéseket az Azure-portálon hello hello.

#### <a name="tooview-existing-backups"></a>tooview meglévő biztonsági másolatok

1. Nyissa meg túl**eszközök** panelen. Jelölje ki, majd kattintson az eszköz. A hello **beállítások** paneljén lépjen túl**felügyeleti > biztonságimásolat-katalógus**.
   
    ![Keresse meg a toobackup katalógus](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. Adja meg a következő feltételek toobe a szűréshez használt hello:
   
    - **Időtartománynak** – lehet **elmúlt 1 óra**, **elmúlt 24 óra**, **elmúlt 7 napban**, **az elmúlt 30 napban**, **múltbeli évre** , és **egyéni dátum**.
    
    - **Eszközök** – válassza ki a fájlkiszolgálók és a StorSimple Device Manager szolgáltatásban regisztrált iSCSI-kiszolgálókhoz hello listából.
   
    - **Kezdeményezett** – automatikusan lehet **ütemezett** (által a biztonsági mentési házirend) vagy **manuálisan** kezdeményeznie (,).
   
    ![Biztonsági mentések szűrése](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. Kattintson az **Alkalmaz** gombra. hello biztonsági mentések szűrt listája megjelenik hello **biztonságimásolat-katalógus** panelen. Megjegyzés: csak 100 biztonsági mentési elemek megjelenítése egy adott időpontban.
   
    ![Frissített biztonságimásolat-katalógus](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a>Következő lépések

További információ [felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).

