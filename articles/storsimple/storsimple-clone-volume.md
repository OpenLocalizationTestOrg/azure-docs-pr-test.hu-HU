---
title: "aaaClone a StorSimple-kötet |} Microsoft Docs"
description: "Ismerteti a hello különböző Klónozás típusok és mikor toouse őket, és elmagyarázza, hogyan használhatja a biztonságimásolat-készlet tooclone egy egyéni köteten."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b5d615f2-02a7-4222-9867-6c0385ce748c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: e98d28db1abeb515ba78ab5860e7c5eba0dfcbb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooclone-a-volume"></a>Hello StorSimple Manager szolgáltatás tooclone kötet használata
[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Áttekintés
a StorSimple Manager szolgáltatás hello **biztonságimásolat-katalógus** oldal megjelenik, amely jönnek létre, ha manuális vagy automatikus biztonsági mentés készül minden hello biztonsági mentés. A lap toolist összes hello biztonsági mentés használja a biztonsági mentési házirend, vagy egy kötetet, jelölje be vagy törölje a biztonsági mentések, vagy használja a biztonsági mentési toorestore vagy kötet klónozása.

![Biztonságimásolat-katalógus lap](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

Ez az oktatóanyag leírja, hogyan használhatja a biztonságimásolat-készlet tooclone az egyéni kötetek. Azt is bemutatja hello különbségének *átmeneti* és *állandó* klónozásával készíti. 

## <a name="create-a-clone-of-a-volume"></a>A kötet klónozhatja
Hello hozhatja létre a klónozott ugyanarra az eszközre, egy másik eszközt vagy akár egy virtuális gépet egy helyi vagy egy felhő-pillanatfelvételt.

#### <a name="tooclone-a-volume"></a>a kötet tooclone
1. A hello StorSimple Manager szolgáltatás lapján, kattintson a hello **biztonságimásolat-katalógus** lapot, és válasszon egy biztonságimásolat-készlet.
2. Bontsa ki a hello biztonságimásolat-készlet tooview hello társított kötetek. Kattintson, és jelöljön ki egy kötetet a hello biztonságimásolat-készlet.
   
     ![Kötet klónozása](./media/storsimple-clone-volume/HCS_Clone.png) 
3. Kattintson a **Klónozás** toobegin hello kijelölt kötet klónozása.
4. Hello Klónozás kötet varázslóban a **név és hely megadása**:
   
   1. Azonosítsa a céleszközön. Ez az hello hely, ahol hello Klónozás létrejön. Hello ugyanarra az eszközre, vagy adjon meg egy másik eszközt. Ha, válasszon olyan kötetet, egyéb felhőszolgáltatók társított (nem Azure) hello legördülő hello figyelt eszköz csak akkor jelenik meg fizikai eszközök beállításainak felsorolása. A virtuális eszközön egyéb felhőszolgáltatók társított kötet nem klónozható.
      
      > [!NOTE]
      > Győződjön meg arról, hogy hello Klónozás szükséges hello kapacitást alacsonyabb, mint a rendelkezésre álló hello céleszközön hello kapacitás.
      > 
      > 
   2. Adja meg a klónozott egyedi kötet nevét. hello név hosszúsága 3 és 127 karakterből állhat.
   3. Hello nyíl ikonra ![nyíl ikon](./media/storsimple-clone-volume/HCS_ArrowIcon.png) tooproceed toohello következő oldalra.
5. A **adja meg a gazdagépekhez, amelyek használhatják ezt a kötetet**:
   
   1. Adjon meg egy hozzáférés-vezérlési rekordot (ACR) hello klón. Adja hozzá egy új ACR, vagy hello meglévő lista lehetőségek közül választhat.
   2. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-clone-volume/HCS_CheckIcon.png)toocomplete hello műveletet.
6. A Klónozás feladat indul, és értesítést fog kapni hello klónozás sikeresen létrehozásakor. Kattintson a **feladat megtekintése** toomonitor hello Klónozás feladat hello **feladatok** lap.
7. Hello után Klónozás feladat befejezése után:
   
   1. Nyissa meg toohello **eszközök** lapot, és válassza hello **Kötettárolók** lapon. 
   2. Válassza ki a klónozott hello forráskötet társított hello kötettároló. A kötetek listáját hello megtekintheti az újonnan létrehozott hello Klónozás.

> [!NOTE]
> Figyelés és az alapértelmezett biztonsági mentés automatikusan le vannak tiltva a klónozott köteten.
> 
> 

Az így létrehozott klón átmeneti másolat. Klónozás típusok kapcsolatos további információkért lásd: [átmeneti és végleges klónok](#transient-vs-permanent-clones).

Ehhez a klónhoz most rendszeres kötet, és minden művelet, amely lehet egy olyan köteten hello Klónozás elérhető lesz. Szüksége lesz tooconfigure ezt a kötetet a biztonsági mentés.

## <a name="transient-vs-permanent-clones"></a>Átmeneti és végleges klónok összehasonlítása
Átmeneti és végleges klónok jönnek létre, csak akkor, ha másik eszközön tooa vannak a Klónozás. Egy adott köteten egy biztonságimásolat-készlet tooa másik eszközről tud klónozni. Ily módon létrehozta másolat van egy *átmeneti* Klónozás. hello átmeneti Klónozás hivatkozások toohello eredeti kötet lesz, és helyileg írása közben a kötet tooread fogja használni. 

Egy felhő-pillanatfelvételt a átmeneti klón elvégzése után hello eredményül kapott Klónozás lesz egy *állandó* Klónozás. hello állandó Klónozás független, és nem rendelkezik a hivatkozások toohello eredeti köteten, amely a klónozása megtörtént.  

## <a name="scenarios-for-transient-and-permanent-clones"></a>Átmeneti és végleges klónok forgatókönyvei
hello a következő szakaszok ismertetik például olyan helyzetekben, ahol átmeneti és végleges klónok is használható.

### <a name="item-level-recovery-with-a-transient-clone"></a>Elemszintű helyreállítás az átmeneti másolat
Egy éves Microsoft PowerPoint bemutatót fájl toorecover van szüksége. A rendszergazda hello biztonsági másolat egy adott időszak azonosítja, és majd szűrők hello kötet. hello rendszergazda ezután hello kötet klónozásával készíti talál, amely a keresett hello fájlt és tooyou biztosítja azt. Ebben a forgatókönyvben egy átmeneti Klónozás szolgál. 

![Videó elérhető](./media/storsimple-clone-volume/Video_icon.png)**Videó elérhető**

a videó bemutatja, hogyan használja a hello klónozást, és állítsa vissza a StorSimple toorecover törölt fájlok, a szolgáltatások toowatch kattintson [Itt](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-hello-production-environment-with-a-permanent-clone"></a>Az állandó másolat hello éles környezetben tesztelése
Tooverify hello éles környezetben tesztelési programhiba van szüksége. Pillanatkép készítése a ehhez a klónhoz felhő hello kötet másolat létrehozásához hello éles környezetben. hello klónozott kötet már független. Ebben a forgatókönyvben egy állandó Klónozás szolgál.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[a StorSimple-kötet visszaállítása egy biztonságimásolat-készlet](storsimple-restore-from-backup-set.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

