---
title: "a StorSimple 8000 Series kötet aaaClone |} Microsoft Docs"
description: "Hello különböző Klónozás típusok és használati ismerteti, és elmagyarázza, hogyan használhatja a biztonságimásolat-készlet tooclone az egyéni kötetek a StorSimple 8000 series eszközön."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/26/2017
ms.author: alkohli
ms.openlocfilehash: 4f7e1f62f17c7b2bd72820a00a5ab87b7e192332
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-in-azure-portal-tooclone-a-volume"></a>Az Azure portál tooclone kötet hello StorSimple Device Manager szolgáltatás használata

## <a name="overview"></a>Áttekintés

Ez az oktatóanyag leírja, hogyan használhatja a biztonságimásolat-készlet tooclone hello segítségével az egyes kötetek **biztonságimásolat-katalógus** panelen. Azt is bemutatja hello különbségének *átmeneti* és *állandó* klónozásával készíti. Ebben az oktatóanyagban hello útmutatások a 3-as vagy újabb verzióját futtató frissítés tooall hello StorSimple 8000 sorozat eszköz érvényesek.

StorSimple Device Manager szolgáltatás hello **biztonságimásolat-katalógus** panelt jeleníti meg, amely jönnek létre, ha manuális vagy automatikus biztonsági mentés készül minden hello biztonsági mentés. A kötet a biztonságimásolat-készlet tooclone a választhatja.

 ![Biztonságimásolat-készlet listája](./media/storsimple-8000-clone-volume-u2/bucatalog.png)

## <a name="considerations-for-cloning-a-volume"></a>A kötet klónozásával kapcsolatos szempontok

Vegye figyelembe a következő információ, ha egy kötet klónozása hello.

- A Klónozás viselkedik hello azonos módon rendszeres kötetként. Minden művelet, amely lehet egy olyan köteten hello Klónozás érhető el.

- Figyelés és az alapértelmezett biztonsági mentés automatikusan le vannak tiltva a klónozott köteten. A biztonsági mentés tooconfigure klónozott kötet kellene.

- Egy helyileg rögzített kötet egy rétegzett kötet klónozták. Ha helyileg rögzített kötet klónozott toobe kell hello, konvertálhatja hello Klónozás tooa helyileg rögzített kötetet, hello klónozási művelet sikeres végrehajtása után. A helyileg egy rétegzett kötet tooa konvertálása információ rögzített kötet, nyissa meg túl[hello kötet típusának módosítása](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).

- Ha a rétegzett toolocally klónozott kötet rögzítve (ha az még átmeneti másolat) klónozása után azonnal tooconvert, hello átalakítás meghiúsul, és hello a következő hibaüzenet jelenik meg:

    `Unable toomodify hello usage type for volume {0}. This can happen if hello volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry hello modify operation.`

    Ez a hiba csak akkor, ha másik eszközön tooa vannak a Klónozás érkezik. Sikeresen konvertálhatja hello kötet toolocally hello átmeneti Klónozás tooa állandó Klónozás először át van rögzítve. Pillanatkép készítése hello átmeneti Klónozás tooconvert felhő azt tooa állandó Klónozás.

## <a name="create-a-clone-of-a-volume"></a>A kötet klónozhatja

Létrehozhat egy klónozott ugyanarra az eszközre, egy másik eszközt vagy akár egy felhőalapú készülék hello segítségével egy helyi vagy felhőalapú pillanatfelvétel.

az alábbi hello eljárás ismerteti, hogyan toocreate másolat hello a katalógus biztonsági másolatot készíteni.  Egy másik megoldásként tooinitiate Klónozás túl toogo**kötetek**, egy köteten, majd kattintson a jobb gombbal a tooinvoke hello helyi menüben válassza ki és **Klónozás**.

Hajtsa végre a következő lépéseket toocreate a kötet másolat hello biztonsági mentési katalógusból hello.

#### <a name="tooclone-a-volume"></a>a kötet tooclone

1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást, és kattintson a **biztonságimásolat-katalógus**.

2. Válassza ki a biztonságimásolat-készletet a következőképpen:
   
   1. Válassza ki azt a hello megfelelő eszközt.
   2. Hello legördülő listában válassza ki hello kötet vagy a biztonsági mentési házirendet hello biztonsági mentés, hogy kívánja-e tooselect.
   3. Adjon meg hello időtartományt.
   4. Kattintson a **alkalmaz** tooexecute ezt a lekérdezést.

    hello hello kijelölt kötet vagy a biztonsági mentési házirenddel társított biztonsági másolatok meg kell jelennie hello listájában biztonsági mentés.
   
    ![Biztonságimásolat-készlet listája](./media/storsimple-8000-clone-volume-u2/bucatalog.png)
     
3. Bontsa ki a hello biztonságimásolat-készlet tooview hello társított kötetek. Ezek a kötetek offline állapotba kell helyezni a hello állomás és az eszköz visszaállítani őket. Hello hello köteteket hozzáférési **kötetek** az eszközt, és hajtsa végre hello panelen lévő lépéseket [kötet offline állapotba](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) tootake offline állapotba.
   
   > [!IMPORTANT]
   > Győződjön meg arról, hogy elvégezte hello kötetek offline hello gazdagépen először hello eszközön hello kötetek offline végrehajtása előtt. Ha nem hajtja végre hello kötetek offline hello állomáson, veremtúlcsordulást okozhat toodata sérülése.
   
4. Keresse meg a visszafelé toohello **biztonságimásolat-katalógus** és jelöljön ki egy kötetet a biztonságimásolat-készletben. Kattintson a jobb gombbal, és hello helyi menüből válassza **Klónozás**.

   ![Biztonságimásolat-készlet listája](./media/storsimple-8000-clone-volume-u2/clonevol3b.png) 

3. A hello **Klónozás** panelen hello a következő lépéseket:
   
    1. Azonosítsa a céleszközön. Ez az hello hely, ahol hello Klónozás létrejön. Hello ugyanarra az eszközre, vagy adjon meg egy másik eszközt.

      > [!NOTE]
      > Győződjön meg arról, hogy hello Klónozás szükséges hello kapacitást alacsonyabb, mint a rendelkezésre álló hello céleszközön hello kapacitás.
       
    2. Adja meg a klónozott egyedi kötet nevét. hello név hosszúsága 3 és 127 karakterből állhat.
      
        > [!NOTE]
        > Hello **Klónozás kötet mint** mező kitöltése **rétegzett** akkor is, ha meg vannak Klónozás egy helyileg rögzített kötet. Ez a beállítás; nem módosítható azonban ha klónozott kötet toobe helyileg rögzített is kell hello, konvertálhatja hello Klónozás tooa helyileg rögzített kötet hello Klónozás sikeres létrehozását követően. A helyileg egy rétegzett kötet tooa konvertálása információ rögzített kötet, nyissa meg túl[hello kötet típusának módosítása](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).
          
    3. A **csatlakozó állomások**, adjon meg egy hozzáférés-vezérlési rekordot (ACR) hello klón. Adja hozzá egy új ACR, vagy hello meglévő lista lehetőségek közül választhat. hello ACR mely állomásokat férhetnek hozzá ehhez a klónhoz határozza meg.
      
        ![Biztonságimásolat-készlet listája](./media/storsimple-8000-clone-volume-u2/clonevol3a.png) 

    4. Kattintson a **Klónozás** toocomplete hello műveletet.

4. A Klónozás feladat indítható, és értesítést hello Klónozás sikeres létrehozását. Kattintson a hello feladat értesítés, vagy válassza a túl**feladatok** panel toomonitor hello Klónozás feladat.

    ![Biztonságimásolat-készlet listája](./media/storsimple-8000-clone-volume-u2/clonevol5.png)

7. Hello Klónozás feladat befejezése után nyissa meg tooyour eszközt, és kattintson a **kötetek**. A kötetek listáját hello, kell megjelennie, amely éppen most hozták létre a hello azonos hello Klónozás hello forráskötet rendelkező kötettároló.

    ![Biztonságimásolat-készlet listája](./media/storsimple-8000-clone-volume-u2/clonevol6.png)

Az így létrehozott klón átmeneti másolat. Klónozás típusok kapcsolatos további információkért lásd: [átmeneti és végleges klónok](#transient-vs-permanent-clones).


## <a name="transient-vs-permanent-clones"></a>Átmeneti és végleges klónok összehasonlítása
Átmeneti klónok jönnek létre, csak akkor, ha tooanother eszköz klónozását. Egy adott köteten egy biztonságimásolat-készlet tooa másik eszközről hello StorSimple Device Manager által felügyelt tud klónozni. hello átmeneti Klónozás hivatkozások toohello adatokat tartalmaz az eredeti kötet hello és adott adatok tooread és írási helyileg hello céleszközön.

Egy felhő-pillanatfelvételt a átmeneti klón elvégzése után hello eredményül kapott Klónozás van egy *állandó* Klónozás. A folyamat során hello adatok másolatát hello felhő jön létre, és hello idő toocopy hello hello adatok mérete határozza meg az adatokat, és hello Azure késések (Ez az az Azure-Azure másolása). A folyamat eltarthat nap tooweeks. hello átmeneti Klónozás állandó másolat válik, és nem rendelkezik a hivatkozások toohello eredeti kötet adatokat a klónozása megtörtént.

## <a name="scenarios-for-transient-and-permanent-clones"></a>Átmeneti és végleges klónok forgatókönyvei
hello a következő szakaszok ismertetik például olyan helyzetekben, ahol átmeneti és végleges klónok is használható.

### <a name="item-level-recovery-with-a-transient-clone"></a>Elemszintű helyreállítás az átmeneti másolat
Egy éves Microsoft PowerPoint bemutatót fájl toorecover van szüksége. A rendszergazda hello biztonsági másolat kezdve azonosítja, és majd szűrők hello kötet. hello rendszergazda ezután hello kötet klónozásával készíti talál, amely a keresett hello fájlt és tooyou biztosítja azt. Ebben a forgatókönyvben egy átmeneti Klónozás szolgál.

### <a name="testing-in-hello-production-environment-with-a-permanent-clone"></a>Az állandó másolat hello éles környezetben tesztelése
Tooverify hello éles környezetben tesztelési programhiba van szüksége. Hello éles környezetben klónozhatja hello kötet, és majd pillanatképet egy felhőalapú, a Klónozás toocreate egy független klónozott köteten. Ebben a forgatókönyvben egy állandó Klónozás szolgál.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[a StorSimple-kötet visszaállítása egy biztonságimásolat-készlet](storsimple-8000-restore-from-backup-set-u2.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

