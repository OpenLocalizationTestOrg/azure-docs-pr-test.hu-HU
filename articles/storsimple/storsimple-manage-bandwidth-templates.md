---
title: "aaaManage a StorSimple sávszélesség sablonok |} Microsoft Docs"
description: "Ismerteti, hogyan toomanage StorSimple sávszélesség sablonok, amelyek lehetővé teszik toocontrol sávszélesség-használat."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 0027b90e-91a5-437d-9bd0-06b05674aa5f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2016
ms.author: alkohli
ms.openlocfilehash: 3f767e985667121e977106e7a1f8e5a3ad25f022
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-storsimple-bandwidth-templates"></a>Használjon hello StorSimple Manager szolgáltatás toomanage StorSimple sávszélesség sablonokat.
## <a name="overview"></a>Áttekintés
A sávszélesség a sablonok segítségével tooconfigure a sávszélesség-használat több idő napi ütemezés tootier hello adatait hello StorSimple eszköz toohello felhő között.

A sávszélesség-szabályozási ütemezések segítségével:

* Adja meg a testreszabott sávszélesség ütemezést attól függően, hogy hello munkaterhelés hálózati módjait.
* Kezelésének központosítása, és több eszközön hello ütemezések könnyen és zökkenőmentes módon használja fel.

> [!NOTE]
> Ez a funkció csak a StorSimple fizikai eszközök és a virtuális eszköz nem érhető el.
> 
> 

A szolgáltatás összes hello sávszélesség sablonok táblázatos formában jelennek meg, és tartalmazza a következő információ hello:

* **Név** – egyedi név, hozzárendelése toohello sávszélességsablon létrehozásakor.
* **Ütemezés** – hello egy adott sávszélesség sablonban található ütemezések száma.
* **Által használt** – hello hello sávszélesség-sablonokkal kötetek számát.

Hello StorSimple Manager szolgáltatás használ **konfigurálása** hello Azure klasszikus portál toomanage sávszélesség sablonok lap.

Is tekinthet meg további információkat toohelp sávszélesség sablonok konfigurálása az:

* Sávszélesség-sablonokkal kapcsolatos kérdések és válaszok
* Gyakorlati tanácsok a sávszélesség-sablonok

## <a name="add-a-bandwidth-template"></a>Sávszélességsablon hozzáadása
Hajtsa végre a következő lépéseket toocreate új sávszélességsablon hello.

#### <a name="tooadd-a-bandwidth-template"></a>tooadd sávszélességsablon
1. A StorSimple Manager szolgáltatás hello **konfigurálása** kattintson **sávszélességsablon hozzáadása/szerkesztése**.
2. A hello **Sávszélességsablon hozzáadása/szerkesztése** párbeszédpanel:
   
   1. A hello **sablon** legördülő listában válassza **hozzon létre új** tooadd egy új sávszélesség-sablont.
   2. Adja meg a sávszélesség-sablon egyedi nevét.
3. Adja meg a **sávszélesség ütemezés**. toocreate ütemezés szerint:
   
   1. Hello legördülő listából válassza ki a hello nap hello hét hello ütemterv van beállítva. Több nap hello előtt hello megfelelő nap hello listában található jelölőnégyzetek bejelölésével választhatja.
   2. Jelölje be hello **minden nap** lehetőséget, ha hello teljes napig érvényes hello ütemezés. Ha ez a beállítás be van jelölve, akkor már nem adhat meg egy **kezdete** vagy egy **befejezésének**. hello ütemezés too11 12:00-kor fut: 59 óra.
   3. Hello legördülő listájából válassza ki a **kezdete**. Ez akkor, ha hello ütemezés megkezdődik.
   4. Hello legördülő listájából válassza ki az **befejezésének**. Ez akkor, ha hello ütemezés leáll.
      
      > [!NOTE]
      > Átfedő ütemezések nem engedélyezettek. Hello kezdési és befejezési időpontjai eredményezi az átfedő ütemezés, ha toothat hatása hiba üzenet jelenik meg.
      > 
      > 
   5. Adja meg a hello **sávszélesség mértékének**. Ez a hello sávszélesség megabit / másodperc (Mbps), a StorSimple eszköz (feltöltések és letöltések) hello felhő érintő műveletek használják. Adjon meg egy 1 és 1000 ennél a mezőnél közötti számot.
   6. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). hello sablon létrehozott hozzá szeretné adni a sávszélesség-sablonok listájának toohello hello szolgáltatás **konfigurálása** lap.
      
      ![Új sávszélesség-sablon létrehozása](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)
4. Kattintson a **mentése** hello lapot, és kattintson a hello alján **Igen** során a rendszer megerősítést kér. Ez menti a hello konfigurációs módosításokat végzett.

## <a name="edit-a-bandwidth-template"></a>A sávszélesség-sablon szerkesztése
Hajtsa végre a következő lépéseket tooedit sávszélességsablon hello.

### <a name="tooedit-a-bandwidth-template"></a>tooedit sávszélességsablon
1. Kattintson a **sávszélességsablon hozzáadása/szerkesztése**.
2. A hello **Sávszélességsablon hozzáadása/szerkesztése** párbeszédpanel:
   
   1. A hello **sablon** legördülő menüben válassza ki egy meglévő sávszélesség toomodify kívánt sablont.
   2. Végezze el a módosításokat. (Ezt módosíthatja hello meglévő beállításokat.)
   3. Kattintson a pipa ikonra hello ![Pipa ikon](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Sávszélesség-sablonok listájának hello hello módosított sablon hello szolgáltatás konfigurálása lapon jelenik meg.
3. toosave módosításait, kattintson a **mentése** hello lap hello alján. Kattintson a **Igen** során a rendszer megerősítést kér.

> [!NOTE]
> Ön nem engedélyezett a módosításokat, ha hello szerkesztett ütemezése átfedi egy létező ütemezési hello sávszélességsablon módosítja, hogy toosave.
> 
> 

## <a name="delete-a-bandwidth-template"></a>Sávszélesség sablon törlése
Hajtsa végre a következő lépéseket toodelete sávszélességsablon hello.

#### <a name="toodelete-a-bandwidth-template"></a>toodelete sávszélességsablon
1. A szolgáltatás hello sávszélesség-sablonok listájának táblázatos hello válassza ki, hogy kívánja-e toodelete hello sablont. A Törlés ikonra (**x**) toohello jobb hello kiválasztott sablon megjelenik. Kattintson a hello **x** ikon toodelete hello sablont.
2. Megerősítést kér bekéri. Kattintson a **OK** tooproceed.

Ha hello sablon bármely kötet(ek) használja, akkor nem engedélyezett toodelete azt. Megjelenik egy hiba üzenet arról, hogy a sablon hello használatban van. Egy hibaüzenet párbeszédpanelén jelenik meg tájékoztatja, hogy minden hello hivatkozások toohello sablon el kell távolítani.

Minden hello hivatkozások toohello sablon törlése hello elérésével **Kötettárolók** lapon, és ezt a sablont használja, hogy használjon egy másik sablont, vagy egy egyéni vagy korlátlan sávszélességet hello kötettárolók módosítása a beállítás. Hello minden hivatkozást el lettek távolítva, törölheti hello sablont.

## <a name="use-a-default-bandwidth-template"></a>Sávszélesség alapértelmezett sablon használata
Alapértelmezett sávszélességsablon valósul meg, és használják kötettárolók alapértelmezett tooenforce sávszélesség vezérlők hello felhő való hozzáféréskor. hello alapértelmezett sablont is szolgál a felhasználók számára, akik a saját sablonok létrehozása készen hivatkozásként. az alapértelmezett sablon hello részleteit a következők:

* **Név** – a korlátlan éjszakák, és a hétvégéket
* **Ütemezés** – a hétfő tooFriday közötti 8 óra és délután 5 óra eszköz ideje 1 MB/s sávszélesség mértéke alkalmazó egy ütemezéssel. hello sávszélesség hello hét hello hátralévő tooUnlimited van beállítva.

hello alapértelmezett sablon szerkeszthető. Ez a sablon (beleértve a szerkesztett verzióit) hello használata kulcsban követhető nyomon.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Egy megadott időpontban elinduló napos sávszélesség sablon létrehozása
Hajtsa végre az ezen eljárás toocreate olyan ütemezés, amely elindítja a megadott időpontban, és minden nap futtatja. Hello példában hello ütemezés a hello reggel 9 óra kezdődik, és amíg Reggel 9 hello következő reggel futtat. Fontos toonote, amely hello kezdő és befejező időpontok egy adott ütemezés is tartalmaznia kell hello azonos 24 órás ütemezhet, és nem terjedhetnek ki több nap. Ha módosítania kell a sávszélesség-sablonok, amelyek több nap több tooset, szüksége lesz toouse egyszerre több ütemezés (hello példa szerint).

#### <a name="toocreate-an-all-day-bandwidth-template"></a>egy napos sávszélességsablon toocreate
1. Hozzon létre egy ütemezést, a hello reggel Reggel 9 és éjfél futtatja.
2. Adjon hozzá egy másik ütemezést. Hello második ütemezés toorun éjfél konfigurálása csak a hello reggel 9 óra.
3. Hello sávszélesség sablon mentéséhez.

hello összetett ütemezés majd indítsa el a egyszerre, és futtassa napos.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Sávszélesség-sablonokkal kapcsolatos kérdések és válaszok
**A Q**. Toobandwidth vezérlők mi történik, amikor Between hello ütemezéseket? (Ütemezés befejeződött, és egy másikat még nem kezdődött meg.)

**A**. Ilyen esetekben nem sávszélesség-vezérlés alkalmazzák. Ez azt jelenti, hogy hello eszközökön használható korlátlan sávszélesség, amikor adatokat toohello felhő rétegezéséhez.

**A Q**. Módosíthatja a sávszélesség-sablonok offline-eszközön?

**A**. Nem fogja tudni toomodify sávszélesség sablonok a kötetek tárolók Ha hello megfelelő eszköz offline állapotban.

**A Q**. Szerkesztheti hello társított kötetek offline módban. Ha a kötettároló társított sávszélességsablon?

**A**. A kötettároló, amelynek a kötetek offline módban társított sávszélességsablon módosíthatja. Vegye figyelembe, hogy kötetek offline módban, amikor nincsenek adatok fog helyezhető el a hello eszköz toohello felhőből.

**A Q**. Törölheti alapértelmezett sablont?

**A**. Bár az alapértelmezett sablon törléséhez nincs egy jó ötlet toodo így. alapértelmezett sablon, többek között a szerkesztett verziók hello használata kulcsban követhető nyomon. nyomkövetési adatok hello elemzésnek és hello folyamán idő, használt tooimprove hello alapértelmezett sablont.

**A Q**. Hogyan meg, meg, hogy a sávszélesség-sablonokat kell-e a módosított toobe?

**A**. Lelassul, vagy egy napon belül több alkalommal fogyasztja hello jelentkezik, hogy kell-e toomodify hello sávszélesség sablonok rendszer indításakor egyik hello hálózati jelent. Ez akkor fordul elő, ha figyelni hello tárolás és a használati hálózati hello i/o-teljesítmény- és hálózati átviteli diagramok megtekintésével.

Hello hálózati átviteli sebességről hello időpontot azonosítása és hello kötettárolók mely hello a hálózati szűk következik be. Ha ez történik, ha az adatok folyamatban van a rétegzett toohello felhő (beszerezni ezeket az információkat az összes kötet tárolókat i/o-teljesítmény az eszköz toocloud), akkor a kötettárolók társított toomodify hello sávszélesség sablonok kell.

Hello módosítása után sablonok használatban van, szüksége lesz toomonitor hello hálózati újra jelentős késések fordulnak elő. Ha ezek továbbra is létezik, akkor szüksége lesz toorevisit a sávszélesség-sablonokat.

**A Q**. Mi történik, ha az eszközön több kötettárolók ütemezi, hogy hozzon létre, de különböző korlátok vonatkoznak tooeach?

**A**. Tegyük fel, hogy rendelkezik-e egy eszközön keresztül a 3 kötettárolók. hello ezekhez a tárolókhoz teljesen kapcsolódó átfedésben vannak. Az egyes tárolókban használt hello sávszélességkorlátok 15 MB/s, 5, 10 kulcsattribútumokkal. Ha i/o előforduló összes hello hello legalább hello 3 sávszélesség korlátja egy időben is alkalmazható, ezek a tárolók: Ebben az esetben az 5 MB/s, ezek i/o-kérelmek megosztás kimenő hello ugyanazon a várólistán.

## <a name="best-practices-for-bandwidth-templates"></a>Gyakorlati tanácsok a sávszélesség-sablonok
Kövesse az alábbi gyakorlati tanácsok a StorSimple eszközhöz:

* Az eszköz tooenable változó sávszélesség-szabályozás hello hálózati átviteli hello eszköz különböző napszakokban hello konfigurálása sávszélesség sablonok. A mentési ütemezések használata esetén a sávszélesség sablonok csúcsidőn hatékonyan használhatják fel a felhőműveletek további hálózati sávszélességet.
* Hello tényleges sávszélességre van szüksége az adott példány hello üzembe helyezési és hello szükséges helyreállítási idő célkitűzése (RTO) hello méretének kiszámításához.

## <a name="next-steps"></a>Következő lépések
További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

