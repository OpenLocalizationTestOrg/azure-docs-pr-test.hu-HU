---
title: "aaaManage StorSimple eszközvezérlők |} Microsoft Docs"
description: "Ismerje meg, hogyan toostop, indítsa újra, állítsa le, vagy visszaállíthatja a StorSimple eszköz tartományvezérlőket."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 4ee989d0-956f-4c14-951e-fd4e490ea09d
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2016
ms.author: alkohli
ms.openlocfilehash: 9a86aa0f4a8fd96c36df206774972602c47a49a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-storsimple-device-controllers"></a>A StorSimple eszköz tartományvezérlők kezelése
## <a name="overview"></a>Áttekintés
Ez az oktatóanyag leírja a StorSimple eszköz tartományvezérlőn végrehajtható hello különböző műveleteket. a StorSimple eszköz hello tartományvezérlőinek redundáns (társközi) vezérlők aktív-passzív konfigurációban. Egy adott időben csak egy vezérlő aktív, és minden hello lemezek és a hálózati műveletek feldolgozása. hello más vezérlő a passzív módban van. Hello aktív vezérlő nem sikerül, ha a passzív vezérlő hello automatikusan aktívvá válik.

Ez az oktatóanyag használatával részletesen toomanage hello eszközvezérlők magában foglalja a:

* **Tartományvezérlők** hello szakasza **karbantartási** hello StorSimple Manager szolgáltatás lapján
* A Windows PowerShell-lel.

Azt javasoljuk, hogy hello eszközvezérlők hello StorSimple Manager szolgáltatással felügyeli. Egy művelet csak a StorSimple Windows PowerShell használatával hajtható végre, ha hello oktatóanyag teszi azt jegyezze fel.

Ez az oktatóanyag elolvasása, után fogja tudni:

* A StorSimple eszköz vezérlő leállítása vagy újraindítása
* Állítsa le a StorSimple eszköz
* A StorSimple eszköz toofactory visszaállítása

## <a name="restart-or-shut-down-a-single-controller"></a>Csak egy vezérlő leállítása vagy újraindítása
A tartományvezérlő újraindítás vagy leállítás nincs szükség a szokásos működésének részeként. Egyetlen eszköz tartományvezérlő leállítás műveletek esetében közös csak abban az esetben egy sikertelen eszköz hardverösszetevő használatához szükséges cserélni. Tartományvezérlő újraindításra is szükség lehet, amelyben teljesítmény túl sok memória használata, vagy egy nem megfelelően működő tartományvezérlő által érintett helyzetben. Szükség lehet a toorestart vezérlő után sikeres vezérlő helyettesíti, ha tooenable kívánja, és ellenőrizze cseréje hello vezérlőt.

Egy eszköz újraindítása nincs zavaró tooconnected kezdeményezőket, feltéve, hogy hello passzív vezérlő érhető el. Ha a passzív tartományvezérlő nem érhető el, vagy ki van kapcsolva, majd indítsa újra a hello aktív vezérlő hello megszűnésének szolgáltatás és az állásidőt eredményezhet.

> [!IMPORTANT]
> * **Egy futó tartományvezérlő kell soha nem kell fizikailag eltávolítani, mert ez adatvesztést redundanciát és az állásidő megnövekedett kockázata eredményezne.**
> * hello következő eljárásra csak toohello StorSimple fizikai eszköz. További információ a hogyan toostart, állítsa le és indítsa újra hello virtuális eszköz, lásd: [hello virtuális eszközhöz](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device).
> 
> 

A StorSimple hello hello StorSimple Manager szolgáltatás vagy a Windows PowerShell a klasszikus Azure portál használatával egyetlen eszközt vezérlő leállítása vagy újraindítása

toomanage a eszközvezérlők a klasszikus Azure-portál hello végre hello a következő lépéseket.

#### <a name="toorestart-or-shut-down-a-controller-in-classic-portal"></a>toorestart vagy egy tartományvezérlő, a klasszikus portálon leállítása
1. Keresse meg a túl**eszközök > karbantartási**.
2. Nyissa meg túl**hardverállapot** , és győződjön meg arról, hogy hello állapota az eszközön mindkét hello vezérlők **kifogástalan**.
   
    ![Ellenőrizze, megfelelő StorSimple eszközvezérlők](./media/storsimple-manage-device-controller/IC766017.png)
3. Hello hello aljáról **karbantartási** kattintson **tartományvezérlők kezelése**.
   
    ![A StorSimple eszköz tartományvezérlők kezelése](./media/storsimple-manage-device-controller/IC766018.png)</br>
   
   > [!NOTE]
   > Ha nem látható **tartományvezérlők kezelése**, majd tooinstall frissítenie kell. További információkért lásd: [a StorSimple eszköz frissítése](storsimple-update-device.md).
   > 
   > 
4. A hello **vezérlő beállítások módosítása** párbeszédpanel mezőbe hello a következő:
   
   1. A hello **válassza ki a tartományvezérlő** legördülő listában, jelölje be hello vezérlő, amelyet az toomanage. hello beállítások esetén a vezérlő 0 és a vezérlő 1. Ezek a tartományvezérlők is aktív vagy passzív azonosítja.
      
      > [!NOTE]
      > A vezérlő nem tudja felügyelni, ha ki van kapcsolva vagy nem érhető el, és már nem jelenik a hello legördülő listában.
      > 
      > 
   2. A hello **művelet kiválasztása** legördülő menüben válassza ki **indítsa újra a tartományvezérlő** vagy **állítsa le a tartományvezérlő**.
      
       ![Indítsa újra a StorSimple eszköz passzív vezérlő](./media/storsimple-manage-device-controller/IC766020.png)
   3. Kattintson a pipa ikonra hello ![Pipa ikon](./media/storsimple-manage-device-controller/IC740895.png).

A rendszer leállítása vagy újraindítása hello vezérlő. hello az alábbi táblázat összefoglalja, mi történik, attól függően, hogy hello megadott beállítások megmaradnak hello hello részleteit **vezérlő beállítások módosítása** párbeszédpanel megnyitásához.  

| Kijelölés # | Ha úgy dönt, hogy... | Ez akkor történik. |
| --- | --- | --- |
| 1. |Indítsa újra a hello passzív vezérlő. |Egy feladat toorestart hello tartományvezérlő jön létre, és értesítést fog kapni hello feladat sikeres létrehozása után. Ez lesz újraindításhoz hello vezérlő. Hello újraindítási folyamatot figyelheti elérésével **szolgáltatás > irányítópult > műveletnaplók megtekintése** és a paraméterek megadott tooyour szolgáltatás majd szűrés. |
| 2. |Indítsa újra a hello aktív vezérlő. |Hello a következő figyelmeztetés jelenik meg: "hello aktív vezérlő újraindítása, ha hello eszköz hajt végre feladatátvételt toohello passzív vezérlő. Szeretné toocontinue?" </br>Ha úgy dönt, ezt a műveletet tooproceed, hello lépések lesz használt azonos toothose toorestart hello passzív vezérlő (lásd a kijelölés 1). |
| 3. |Állítsa le a hello passzív vezérlő. |Hello a következő üzenet jelenik meg: "leállítási befejezése után, szüksége lesz a toopush hello power gombra a vezérlő tooturn azt meg. Biztos, hogy azt szeretné, hogy a tartományvezérlő le tooshut?" </br>Ha úgy dönt, ezt a műveletet tooproceed, hello lépések lesz használt azonos toothose toorestart hello passzív vezérlő (lásd a kijelölés 1). |
| 4. |Állítsa le a hello aktív vezérlő. |Hello a következő üzenet jelenik meg: "leállítási befejezése után, szüksége lesz a toopush hello power gombra a vezérlő tooturn azt meg. Biztos, hogy azt szeretné, hogy a tartományvezérlő le tooshut?" </br>Ha úgy dönt, ezt a műveletet tooproceed, hello lépések lesz használt azonos toothose toorestart hello passzív vezérlő (lásd a kijelölés 1). |

#### <a name="toorestart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>toorestart vagy egy tartományvezérlő, a Windows PowerShellben a StorSimple Leállítás
Hajtsa végre a következő lépések tooshut le hello, vagy indítsa újra a klasszikus Azure portálon hello a StorSimple eszköz csak egy vezérlő.

1. Hello eszközt hello soros konzol vagy egy telnet-munkamenet távoli számítógépről. Csatlakozzon a tooController 0, vagy a vezérlő 1 következő hello által ismertetett visszaállítási lépésekkel [PuTTY használata tooconnect toohello eszköz soros konzoljához](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.
3. A szalagcím üdvözlőüzenetére, jegyezze fel a hello vezérlő kapcsolódik-e túl (vezérlő 0 vagy 1 tartományvezérlő), és hogy-e hello aktív vagy passzív (készenléti állapotban lévő) tartományvezérlők hello.
   
   * tooshut le egyetlen tartományvezérlő, írja be hello parancssorba:
     
       `Stop-HcsController`
     
       Ez csatlakozik hello vezérlőnek le fog állni. Ha megszakítja az aktív vezérlő hello, majd azt hajt végre feladatátvételt toohello passzív vezérlő után állítja le.
   * toorestart egy tartományvezérlő, hello parancssorba írja be:
     
       `Restart-HcsController`
     
       Ez újra fog indulni hello vezérlő, amely csatlakozik. Hello aktív vezérlő újraindítása után, akkor hajt végre feladatátvételt toohello passzív vezérlő hello újraindítása előtt.

## <a name="shut-down-a-storsimple-device"></a>Állítsa le a StorSimple eszköz
Ez a szakasz azt ismerteti, hogyan tooshut le egy futó vagy egy távoli számítógépről nem sikerült a StorSimple eszköz. Egy eszköz mindkét hello eszközvezérlők leállítása után ki van kapcsolva. Egy eszköz leállítás történik, fizikai mozgatására, vagy nem működik lesz végrehajtva hello eszközökhöz.

> [!IMPORTANT]
> Hello eszköz leállítása előtt ellenőrizze az eszköz összetevők hello hello állapotát. Keresse meg a túl**eszközök > karbantartási > hardverállapot** , és győződjön meg arról, hogy az összes hello összetevő hello LED állapota zöld. Csak a megfelelő eszköz egy zöld állapot lesz. Ha az eszköz tooreplace hibás összetevő leállt lesznek, látni fogja a sikertelen (piros), vagy egy csökkent (sárga) állapotát hello megfelelő összetevője.
> 
> 

#### <a name="tooshut-down-a-storsimple-device"></a>a StorSimple eszköz le tooshut
1. Használjon hello [vezérlő leállítása vagy újraindítása](#restart-or-shut-down-a-single-controller) eljárás tooidentify és leállítási hello passzív vezérlő az eszközön. Hajtható végre ez a művelet hello a klasszikus Azure portálon vagy a Windows PowerShell StorSimple.
2. Ismételje meg a fenti lépés tooshut hello aktív vezérlő le hello.
3. Most szüksége lesz: hello toolook vissza vezérlősík hello eszköz. Miután hello két vezérlő teljesen állnak le, hello állapot LED mindkét hello tartományvezérlőn kell kell villogó piros. Ha tooturn hello eszköz ki teljesen jelenleg, tükrözés hello power Power-és hűtési modulok (PCMs) vált toohello OFF pozícióját. Ez kapcsolja ki hello eszköz.

<!--#### tooshut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect toohello serial console of hello StorSimple device by following hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In hello serial console menu, verify from hello banner message that hello controller you are connected toois hello passive controller. If you are connected toohello active controller, disconnect from this controller and connect toohello other controller.

1. In hello serial console menu, choose option 1, **log in with full access**.

1. At hello prompt, type:

    `Stop-HCSController`

    This should shut down hello current controller. tooverify whether hello shutdown has finished, check hello back of hello device. hello controller status LED should be solid red.

1. Repeat steps 1 through 4 tooconnect toohello active controller and then shut it down.

1. After both hello controllers are completely shut down, hello status LEDs on both should be blinking red. If you need tooturn off hello device completely at this time, flip hello power switches on both Power and Cooling Modules (PCMs) toohello OFF position.-->

## <a name="reset-hello-device-toofactory-default-settings"></a>Hello eszköz toofactory alapértelmezett beállításainak alaphelyzetbe állítása
> [!IMPORTANT]
> Ha tooreset toofactory alapértelmezett beállításait, forduljon a Microsoft Support. Az alábbiakban hello eljárás csak a Microsoft Support párhuzamosan használható.
> 
> 

Ez az eljárás ismerteti, hogyan tooreset a Microsoft Azure StorSimple eszköz toofactory alapértelmezett beállítások a StorSimple Windows PowerShell használatával.
Gyári beállítások visszaállításával a eltávolítja minden adatot és beállítást hello fürtözési alapértelmezés szerint.

Hajtsa végre a következő lépéseket tooreset hello a Microsoft Azure StorSimple eszköz toofactory alapértelmezett beállítások:

### <a name="tooreset-hello-device-toodefault-settings-in-windows-powershell-for-storsimple"></a>tooreset hello eszköz toodefault beállításait a Windows PowerShell StorSimple
1. Hello eszközt a soros konzolon keresztül. Ellenőrizze, hogy-e aktív vezérlő csatlakoztatott toohello hello szalagcím üzenet tooensure.
2. Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.
3. Hello parancssorba írja be a következő parancs tooreset hello fürtözési, minden adatokat, a metaadatok és a tartományvezérlő-beállítások eltávolítása hello:
   
    `Reset-HcsFactoryDefault`
   
    tooinstead alaphelyzetbe állítani egy tartományvezérlő, használja a hello [alaphelyzetbe állítása-HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) hello parancsmagot `-scope` paraméter.)
   
    hello rendszer többször újraindul. Ön értesítést fog kapni hello alaphelyzetbe állítása sikeresen befejeződött. Attól függően, hogy hello rendszermodell 45-60 perces 8100-eszköz és a 60-90 percig, amíg egy 8600 toofinish ezt a folyamatot is tarthat.
   
   > [!TIP]
   > * Ha ezt 1.2-es frissítés használ, vagy korábbi hello `–SkipFirmwareVersionCheck` paraméter tooskip hello belső vezérlőprogram verziójának ellenőrzése (ellenkező esetben látni fogja, a belső vezérlőprogram verzióeltérési hiba: a gyári beállítások visszaállítása hello belsővezérlőprogram-verziók tooa eltérése miatt nem lehet folytatni. ).
   > * hello gyári alaphelyzetbe állítása eljárás sikertelen lehet a hello kormányzati portálon Update 1 vagy 1.1 futtatja, és végeztek el (a helyettesítő vezérlők, a frissítés előtti 1 teljesített sikeres egyszeres vagy kettős vezérlő helyettesíti a StorSimple eszközök szoftver). Ez történik, ha hello gyári kép hello jelenlét hello vezérlő nem létezik a frissítés előtti 1 szoftver SHA1 fájl van hitelesítve. Ha megjelenik a gyári alaphelyzetbe állítja a hiba, forduljon a Microsoft Support tooassist hello kapcsolatos további lépések. A probléma nem látják az 1. frissítés vagy újabb szoftvert hello gyárból származó teljesített helyettesítő vezérlők.
   > 
   > 

## <a name="questions-and-answers-about-managing-device-controllers"></a>Kérdések és válaszok eszközvezérlők kezelése
Ez a szakasz azt kell foglalja össze néhány gyakori kérdés hello kezelése StorSimple eszközvezérlők tekintetében.

**K.** Mi történik, ha mindkét hello, tartományvezérlői meg az eszköz megfelelő és bekapcsolva és I leállítása vagy újraindítása hello aktív vezérlő?

**V.** Ha mindkét hello tartományvezérlők, az eszközön működik megfelelően, és kikapcsolt, kérni fogja megerősítést kér. Ki is:

* **Indítsa újra az aktív vezérlő hello** – értesítést fog kapni, hogy az aktív vezérlőhöz újraindul, akkor hello eszköz toofail toohello passzív vezérlő fölé. hello controller újraindításához.
* **Állítsa le az aktív vezérlőhöz** – értesítést fog kapni, hogy az aktív vezérlőhöz leállítása leállást eredményez. Hello eszköz tooturn hello tartományvezérlőn toopush hello főkapcsoló is szüksége lesz.

**K.** Mi történik, ha hello passzív vezérlő meg az eszköz nem érhető el, és be van kapcsolva a kikapcsolva, és indítsa újra, vagy állítsa le a hello aktív vezérlő?

**V.** Ha az eszközön a hello passzív vezérlő ki van kapcsolva vagy nem érhető el, és úgy dönt, hogy:

* **Indítsa újra az aktív vezérlő hello** – értesítést fog kapni, hogy hello művelet folytatása a ideiglenes szüneteltetése hello szolgáltatást fog eredményezni, és kérni fogja a megerősítő.
* **Állítsa le az aktív vezérlőhöz** – értesítést fog kapni, hogy hello művelet folytatása állásidőt, és az eredményezi, hogy szüksége lesz a hello eszközön legalább az egyik tartományvezérlők tooturn toopush hello főkapcsoló. Megerősítést kér bekéri.

**K.** Ha nem hello tartományvezérlő újraindítása vagy leállítása sikertelen tooprogress?

**V.** A vezérlő leállítása és újraindítása sikertelen lehet, ha:

* Egy eszköz frissítése folyamatban van.
* A tartományvezérlő újraindítása már folyamatban van.
* Egy tartományvezérlő leállítás már folyamatban van.

**K.** Hogyan meg kitalálja, hogy ha egy tartományvezérlő újraindult, vagy állítsa le?

**V.** Hello tartományvezérlő állapotát hello karbantartási oldalon ellenőrizheti. hello tartományvezérlő állapotát jelzi, hogy a tartományvezérlő újraindítása vagy leállítása. Emellett hello riasztások lapon fogja tartalmazni az információs riasztások, ha hello vezérlő lett újraindult, vagy állítsa le. hello vezérlő újraindítás és leállítás műveletek hello műveletnaplók is rögzíti. A műveletnaplók kapcsolatos további információkért lásd az túl[hello műveletnaplók megtekintése](storsimple-service-dashboard.md#view-the-operations-logs).

**K.** Nincs semmilyen hatása toohello i/o vezérlő feladatátvétel miatt?

**V.** TCP-kapcsolatok hello kezdeményezők és az aktív vezérlő között vezérlő feladatátvétel miatt vissza lesz állítva, de hozni, ha a passzív vezérlő hello azt feltételezi, hogy a művelet. Valószínűleg i/o-tevékenységben kezdeményezők és hello eszköz között (kevesebb, mint 30 másodperc) ideiglenes szünetet hello Ez a művelet során.

**K.** Hogyan visszatérési meg a vezérlő tooservice, leállítása és eltávolítása után?

**V.** a vezérlő tooservice tooreturn, kell behelyezi hello váz leírtak [cserélje le a StorSimple eszköz vezérlő modul](storsimple-controller-replacement.md).

## <a name="next-steps"></a>Következő lépések
* Ha problémába ütközik a StorSimple eszköz tartományvezérlőket ebben az oktatóanyagban szereplő hello eljárások használatával nem oldható fel a [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md).
* hello StorSimple Manager szolgáltatás, használatával kapcsolatban további toolearn lépjen túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

