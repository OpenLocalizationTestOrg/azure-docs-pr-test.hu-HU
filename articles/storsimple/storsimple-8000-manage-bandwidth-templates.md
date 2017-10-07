---
title: "a StorSimple 8000 series aaaManage sávszélesség sablonok |} Microsoft Docs"
description: "Ismerteti, hogyan toomanage StorSimple sávszélesség sablonok, amelyek lehetővé teszik toocontrol sávszélesség-használat."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: ebcd1824d7bb9e4c235194c04edbfe8001a3e794
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-storsimple-bandwidth-templates"></a>Használjon hello StorSimple Device Manager szolgáltatás toomanage StorSimple sávszélesség sablonokat.

## <a name="overview"></a>Áttekintés

A sávszélesség a sablonok segítségével tooconfigure a sávszélesség-használat több idő napi ütemezés tootier hello adatait hello StorSimple eszköz toohello felhő között.

A sávszélesség-szabályozási ütemezések segítségével:

* Adja meg a testreszabott sávszélesség ütemezést attól függően, hogy hello munkaterhelés hálózati módjait.
* Kezelésének központosítása, és több eszközön hello ütemezések könnyen és zökkenőmentes módon használja fel.

> [!NOTE]
> Ez a funkció csak a StorSimple fizikai eszközök (modellek 8100 és 8600) és a StorSimple felhő készülékek (modellek 8010-es és a 8020-as modell) nem érhető el.


## <a name="hello-bandwidth-templates-blade"></a>hello sávszélesség sablonok panel

Hello **sávszélesség sablonok** panelen táblázatos formában a szolgáltatás az összes hello sávszélesség sablonok rendelkezik, és tartalmazza a következő információ hello:

* **Név** – egyedi név, hozzárendelése toohello sávszélességsablon létrehozásakor.
* **Ütemezés** – hello egy adott sávszélesség sablonban található ütemezések száma.
* **Által használt** – hello hello sávszélesség-sablonokkal kötetek számát.

Is tekinthet meg további információkat toohelp sávszélesség sablonok konfigurálása az:

* [Sávszélesség-sablonokkal kapcsolatos kérdések és válaszok](#questions-and-answers-about-bandwidth-templates)
* [Gyakorlati tanácsok a sávszélesség-sablonok](#best-practices-for-bandwidth-templates)

## <a name="add-a-bandwidth-template"></a>Sávszélességsablon hozzáadása

Hajtsa végre a következő lépéseket toocreate új sávszélességsablon hello.

#### <a name="tooadd-a-bandwidth-template"></a>tooadd sávszélességsablon

1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást, kattintson a **sávszélesség sablonok** majd **+ Hozzáadás sávszélességsablon**.

    ![Kattintson a + sávszélességsablon hozzáadása](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp1.png)

2. A hello **sávszélességsablon hozzáadása** panelen hello a következő lépéseket:
   
    1. Adja meg a sávszélesség-sablon egyedi nevét.
    2. Sávszélesség ütemezés meghatározása. toocreate ütemezés szerint:
   
        1. Hello legördülő listából válassza ki a hello **nap** hello hét hello az ütemezés beállítása. Kiválaszthatja, hogy több nap.        
        
        2. Adjon meg egy **kezdete** a _óó: pp_ formátumban. Ez akkor, ha hello ütemezés megkezdődik.

        3. Adjon meg egy **befejezésének** a _óó: pp_ formátumban. Ez akkor, ha hello ütemezés leáll.
      
           > [!NOTE]
           > Átfedő ütemezések nem engedélyezettek. Hello kezdési és befejezési időpontjai eredményezi az átfedő ütemezés, ha toothat hatása hiba üzenet jelenik meg.

        4. Adja meg a hello **sávszélesség mértékének**. Ez a hello sávszélesség megabit / másodperc (Mbps), a StorSimple eszköz (feltöltések és letöltések) hello felhő érintő műveletek használják. Adjon meg egy 1 és 1000 ennél a mezőnél közötti számot.

            ![Sávszélesség ütemezés megadása](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp2.png)
         
            Ismételje meg a fent lépéseket toodefine hello egyszerre több ütemezés a sablon amíg végzett.

        5. Kattintson a **Hozzáadás** toostart sávszélesség sablon létrehozásához. sávszélesség-sablonok listájának toohello létrehozott sablon hello jelenik meg.
      

## <a name="edit-a-bandwidth-template"></a>A sávszélesség-sablon szerkesztése

Hajtsa végre a következő lépéseket tooedit sávszélességsablon hello.

### <a name="tooedit-a-bandwidth-template"></a>tooedit sávszélességsablon

1. Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **sávszélesség sablonok**.
2. Sávszélesség-sablonok listájának hello válassza ki kívánja toodelete hello sablont. Kattintson a jobb gombbal, és hello helyi menüből válassza ki a **törlése**.
3. Amikor felszólítja a megerősítésre, kattintson **OK**. Ez a hello sávszélességsablon törölni kell. 
4. sávszélesség-sablonok listájának hello tooreflect hello törlés frissíti.

> [!NOTE]
> A módosítások nem menthetők, ha hello szerkeszt hello sávszélesség sablont, amely módosítja a meglévő ütemezés ütemezés átfedésben.

## <a name="delete-a-bandwidth-template"></a>Sávszélesség sablon törlése

Hajtsa végre a következő lépéseket toodelete sávszélességsablon hello.

#### <a name="toodelete-a-bandwidth-template"></a>toodelete sávszélességsablon

1. Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **sávszélesség sablonok**.
2. Sávszélesség-sablonok listájának hello válassza ki kívánja toodelete hello sablont. Kattintson a jobb gombbal, és hello helyi menüből válassza a törlés.
3. Amikor felszólítja a megerősítésre, kattintson **OK**. Ez a hello sávszélességsablon törölni kell.
4. sávszélesség-sablonok listájának hello tooreflect hello törlés frissíti.

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

További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

