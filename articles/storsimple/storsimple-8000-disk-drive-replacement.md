---
title: "a StorSimple 8000 series eszközön lemezmeghajtó aaaReplace |} Microsoft Docs"
description: "Ismerteti, hogyan tooreplace lemez meghajtója StorSimple elsődleges ház vagy egy EBOD ház."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 073/2017
ms.author: alkohli
ms.openlocfilehash: 09c1bf5e97adf54a609a868e921351bc63e00a83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-disk-drive-on-your-storsimple-8000-series-device"></a>Cserélje le a lemezmeghajtó a StorSimple 8000 series eszközön

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag azt ismerteti, hogyan távolítsa el, és cserélje le a hibás vagy nem sikerült merevlemez-meghajtó a Microsoft Azure StorSimple eszközön. a meghajtó tooreplace, kell:

* Hello antitamper zárolási működésűeknek
* Távolítsa el a hello a lemezmeghajtó
* Hello helyettesítő meghajtó telepítése

> [!IMPORTANT]
> Mielőtt eltávolítása és cseréje egy meghajtó, tekintse át a biztonsági információk hello [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).
 

## <a name="disengage-hello-antitamper-lock"></a>Hello antitamper zárolási működésűeknek
Ez az eljárás azt ismerteti, hogyan hello antitamper zárolásokat a StorSimple eszköz is lehet vagy kikapcsolt állapotban hello lemezmeghajtók cseréjekor. hello antitamper zárolások szerelnek hello meghajtó szolgáltatónként kezelik, és azokat egy kis nyílás hello zárolás szakaszában hello leíró keresztül érik el. Meghajtók hello zárolások beállított zárolt toohello pozíciót végzik.

#### <a name="toounlock-hello-antitamper-lock"></a>toounlock hello antitamper zárolása
1. Gondosan beszúrása hello zárolási kulcs ("tamperproof" T10 csavarhúzót Microsoft biztosított), a hello leíró hello nyílás és a szoftvercsatorna. 
   
   Ha hello antitamper zárolási aktív, piros hello mutató hello nyílás látható.
  
    ![Zárolt meghajtó](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    **1. ábra** részt vevő elleni illetéktelen zárolása
   
   | Címke | Leírás |
   |:--- |:--- |
   | 1 |Kijelző nyílás |
   | 2 |Antitamper zárolása |
2. Forgassa el hello kulcs anticlockwise irányba, amíg a piros hello mutató nem látható a fenti hello kulcs hello nyílás.
3. Távolítsa el a hello kulcsot.
   
    ![Nem zárolt meghajtó](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    **2. ábra** feloldva lemezmeghajtó
4. hello a lemezmeghajtó most eltávolítható.

Hello lépésekkel fordított tooengage hello zárolva.

## <a name="remove-hello-disk-drive"></a>Távolítsa el a hello a lemezmeghajtó
A StorSimple eszköz támogatja a RAID 10-szerű szóközöket tárkonfiguráció. Ez azt jelenti, hogy azt is megfelelően működik egy meghibásodott lemezt, SSD-meghajtót (SSD), vagy a merevlemez-meghajtó (HDD).

> [!IMPORTANT]
> * Ha a rendszer több mint egy meghibásodott lemezt, távolítsa el egynél több SSD és HDD hello rendszer bármikor időben. Így adatvesztést eredményezhet.
> * Ellenőrizze, hogy egy helyettesítő SSD helyez egy SSD korábban tartalmazó tárhely. Hasonlóképpen helyezze HDD helyettesíti, amely korábban tartalmazott egy HDD tárolóhelyen található.
> * Hello Azure-portálon, a bővítőhelyek számozása 0 – 11. Ezért ha hello portal jeleníti meg, hogy egy lemezt a tárolóhely 2 rendelkezik, az eszközön nem sikerült hello, keresse meg hello harmadik tárolóhely hello meghibásodott lemez a bal felső hello.
> 
> 

Meghajtók távolítva, és amíg hello rendszer cserélni.

#### <a name="tooremove-a-drive"></a>a meghajtó tooremove
1. tooidentify hello nem sikerült a lemezt, hello Azure-portálon lépjen tooyour eszköz **beállítások > hardver állapotának**. Hello lemezei hello állapotának tekintse meg a lemez sikertelen lehet hello elsődleges ház és/vagy egy EBOD ház (Ha egy 8600 modellt használ), mert **összetevők megosztott** és a **EBOD megosztott összetevők** . A hibás lemez vagy szolgáltatással vörös állapottal jelenik meg.
2. Keresse meg hello meghajtók hello elsődleges ház vagy hello EBOD ház hello elejéhez. 
3. Hello lemez fel oldva, folytassa a következő lépésre toohello műveletet. Hello lemez le van tiltva, ha a zárolást a hello eljárást követve [hello antitamper zárolási működésűeknek](#disengage-the-antitamper-lock).
4. Nyomja le az hello fekete hello meghajtó szolgáltatónként modul zárolni, és hello meghajtó szolgáltatónként leíró számítógépnél ki és lekéréses hello Váztípus hello elölről.
   
    ![Lemezmeghajtó leíró feloldása](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    **3. ábra** felszabadításával hello meghajtó leíró
5. Ha teljesen ki van bővítve a hello meghajtó szolgáltatónként leíró, csúsztassa be hello meghajtó szolgáltatónként hello váz kívül. 
   
    ![A késleltetett lemez Lemezmeghajtó kívül](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    **4. ábra** késleltetett hello meghajtó kívül hello szolgáltatója

## <a name="install-hello-replacement-disk-drive"></a>Hello helyettesítő meghajtó telepítése
Miután egy meghajtót a StorSimple eszközt a sikertelen volt, és törölte, hajtsa végre az ezen eljárás tooreplace azt egy új meghajtót.

#### <a name="tooinsert-a-drive"></a>a meghajtó tooinsert
1. Győződjön meg arról, hello meghajtó szolgáltatónként leíró teljesen ki van bővítve, ahogy az a következő kép hello.
   
    ![A kiterjesztett leíró meghajtó](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    **5. ábra** kiterjesztett leíró rendelkező meghajtóra
2. Csúsztassa be a hello meghajtó szolgáltatónként összes hello módon történő hello váz.
   
    ![A meghajtó szolgáltatónként mozgó lemez](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    **6. ábra** csúszó hello meghajtó szolgáltatónként hello váz be
3. Hello meghajtó vivőjel zárja be, beszúrt hello meghajtó szolgáltatónként leírójú folyamatos toopush hello meghajtó szolgáltatónként hello váz, amíg nem hello meghajtó szolgáltatónként leíró zárolt helyzetbe illeszkedik az közben.
4. Hello zárolási kulcs használata által biztosított Microsoft (tamperproof Torx csavarhúzóra) toosecure hello szolgáltatónként leíró helyen bekapcsolásával hello zárolási csavart negyedéves kapcsolja jobbra.
5. Ellenőrizze, hogy sikeres volt-e hello csere és hello meghajtó működési. Hello Azure-portál eléréséhez, és keresse meg a túl**beállítások** > **hardver állapotának**. A **összetevők megosztott** vagy **EBOD megosztott összetevők**, hello meghajtó állapota zöld, jelezve, hogy kifogástalan kell lennie.
<!---Loc Comment: It seems it should say "Device settings > Hardware health" instead of "Settings > Hardware health"---->
   
   > [!NOTE]
   > Ez több óráig is eltarthat a hello lemez állapota tooturn zöld hello csere utáni.
  
## <a name="next-steps"></a>Következő lépések
További információ [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).

