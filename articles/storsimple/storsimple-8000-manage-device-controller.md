---
title: "aaaManage a StorSimple 8000 series eszközvezérlők |} Microsoft Docs"
description: "Ismerje meg, hogyan toostop, indítsa újra, állítsa le, vagy visszaállíthatja a StorSimple eszköz tartományvezérlőket."
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
ms.date: 06/19/2017
ms.author: alkohli
ms.openlocfilehash: 5c59582b7ccf7cfeae9e7efbd0e4df9dc1d3871c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-storsimple-device-controllers"></a>A StorSimple eszköz tartományvezérlők kezelése

## <a name="overview"></a>Áttekintés

Ez az oktatóanyag leírja a StorSimple eszköz tartományvezérlőn végrehajtható hello különböző műveleteket. a StorSimple eszköz hello tartományvezérlőinek redundáns (társközi) vezérlők aktív-passzív konfigurációban. Egy adott időben csak egy vezérlő aktív, és minden hello lemezek és a hálózati műveletek feldolgozása. hello más vezérlő a passzív módban van. Hello aktív vezérlő meghibásodásakor hello passzív vezérlő automatikusan aktiválódik.

Ez az oktatóanyag használatával részletesen toomanage hello eszközvezérlők magában foglalja a:

* **Tartományvezérlők** panel hello StorSimple Device Manager szolgáltatás az eszközt.
* A Windows PowerShell-lel.

Azt javasoljuk, hogy hello eszközvezérlők hello StorSimple Device Manager szolgáltatáson keresztül felügyelt. Egy művelet csak a StorSimple Windows PowerShell használatával hajtható végre, ha hello oktatóanyag teszi azt jegyezze fel.

Ez az oktatóanyag elolvasása, után fogja tudni:

* A StorSimple eszköz vezérlő leállítása vagy újraindítása
* Állítsa le a StorSimple eszköz
* A StorSimple eszköz toofactory visszaállítása

## <a name="restart-or-shut-down-a-single-controller"></a>Csak egy vezérlő leállítása vagy újraindítása
A tartományvezérlő újraindítás vagy leállítás nincs szükség a szokásos működésének részeként. Egyetlen eszköz tartományvezérlő leállítás műveletek esetében közös csak abban az esetben egy sikertelen eszköz hardverösszetevő használatához szükséges cserélni. Tartományvezérlő újraindításra is szükség lehet, amelyben teljesítmény túl sok memória használata, vagy egy nem megfelelően működő tartományvezérlő által érintett helyzetben. Szükség lehet a toorestart vezérlő után sikeres vezérlő helyettesíti, ha tooenable kívánja, és ellenőrizze cseréje hello vezérlőt.

Egy eszköz újraindítása nincs zavaró tooconnected kezdeményezőket, feltéve, hogy hello passzív vezérlő érhető el. Ha a passzív tartományvezérlő nem érhető el, vagy ki van kapcsolva, majd indítsa újra a hello aktív vezérlő hello megszűnésének szolgáltatás és az állásidőt eredményezhet.

> [!IMPORTANT]
> * **Egy futó tartományvezérlő kell soha nem kell fizikailag eltávolítani, mert ez adatvesztést redundanciát és az állásidő megnövekedett kockázata eredményezne.**
> * hello következő eljárásra csak toohello StorSimple fizikai eszköz. További információ a hogyan toostart, állítsa le és indítsa újra hello StorSimple felhő készüléknek, lásd: [hello felhő készülék együttműködve](storsimple-8000-cloud-appliance-u2.md##work-with-the-storsimple-cloud-appliance).

Indítsa újra, vagy állítsa le a egyetlen eszközt vezérlő hello hello StorSimple Device Manager szolgáltatás vagy a Windows PowerShell Azure-portálon keresztül a StorSimple.

toomanage a eszközvezérlők hello Azure-portálon való végrehajtása hello a következő lépéseket.

#### <a name="toorestart-or-shut-down-a-controller-in-azure-portal"></a>toorestart vagy egy Azure-portálon tartományvezérlő Leállítás
1. A StorSimple eszköz Manager szolgáltatást, lépjen túl**eszközök**. Válassza ki az eszköz a hello eszközök. 

    ![Egy eszköz kiválasztása](./media/storsimple-8000-manage-device-controller/manage-controller1.png)

2. Nyissa meg túl**beállítások > tartományvezérlői**.
   
    ![Ellenőrizze, megfelelő StorSimple eszközvezérlők](./media/storsimple-8000-manage-device-controller/manage-controller2.png)
3. A hello **tartományvezérlők** panelen, győződjön meg arról, hogy hello állapota az eszközön mindkét hello vezérlők **kifogástalan**. Válassza ki a vezérlő. Kattintson a jobb gombbal, majd válassza ki **indítsa újra a** vagy **leállítása**.

    ![StorSimple eszközvezérlők leállítása vagy újraindítása kiválasztása](./media/storsimple-8000-manage-device-controller/manage-controller3.png)

4. Egy feladat létrehozásakor toorestart vagy állítsa le a hello, tartományvezérlői és lehetősége lesz vonatkozó figyelmeztetésekkel fejeződött be, ha van ilyen. toomonitor hello újraindítása vagy leállítása, nyissa meg túl**szolgáltatás > tevékenységi naplóit** és szűréssel paraméterek adott tooyour szolgáltatás. Ha a tartományvezérlő le volt állítva, akkor szüksége lesz a toopush hello power gomb tooturn hello vezérlő tooturn azt meg.

#### <a name="toorestart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>toorestart vagy egy tartományvezérlő, a Windows PowerShellben a StorSimple Leállítás
Hajtsa végre a következő lépések tooshut le hello, vagy indítsa újra a Windows PowerShell hello a StorSimple eszköz csak egy vezérlő StorSimple.

1. Hello eszközt hello soros konzol vagy egy telnet-munkamenet távoli számítógépről. tooconnect tooController 0 vagy 1 vezérlő, kövesse hello [PuTTY használata tooconnect toohello eszköz soros konzoljához](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.
3. A szalagcím üdvözlőüzenetére, jegyezze fel a hello vezérlő kapcsolódik-e túl (vezérlő 0 vagy 1 tartományvezérlő), és hogy-e hello aktív vagy passzív (készenléti állapotban lévő) tartományvezérlők hello.
   
   * tooshut le egyetlen tartományvezérlő, írja be hello parancssorba:
     
       `Stop-HcsController`
     
       Ezzel leállítja hello vezérlő, amely csatlakozik. Ha megszakítja az aktív vezérlő hello, majd hello eszköz feladatátadás toohello passzív vezérlő.

   * toorestart egy tartományvezérlő, hello parancssorba írja be:
     
       `Restart-HcsController`
     
       Az újraindítást hello vezérlő, amely csatlakozik. Hello aktív vezérlő újraindítása, ha az nem keresztül toohello passzív vezérlő hello újraindítása előtt.

## <a name="shut-down-a-storsimple-device"></a>Állítsa le a StorSimple eszköz

Ez a szakasz azt ismerteti, hogyan tooshut le egy futó vagy egy távoli számítógépről nem sikerült a StorSimple eszköz. Egy eszköz ki van kapcsolva, miután mindkét hello eszközvezérlők állnak le. Egy eszköz leállítás történik, fizikai mozgatására, vagy nem működik lesz végrehajtva hello eszközök.

> [!IMPORTANT]
> Hello eszköz leállítása előtt ellenőrizze az eszköz összetevők hello hello állapotát. Keresse meg a tooyour eszközt, és kattintson a **beállítások > hardver állapotának**. A hello **állapotát és a hardver állapotának** panelen, győződjön meg arról, hogy az összes hello összetevő hello LED állapota zöld. Csak a megfelelő eszköz zöld állapotba került. Ha az eszköz tooreplace hibás összetevő leállt lesznek, látni fogja a sikertelen (piros), vagy egy csökkent (sárga) állapotát hello megfelelő összetevője.


#### <a name="tooshut-down-a-storsimple-device"></a>a StorSimple eszköz le tooshut

1. Használjon hello [vezérlő leállítása vagy újraindítása](#restart-or-shut-down-a-single-controller) eljárás tooidentify és leállítási hello passzív vezérlő az eszközön. Hajtható végre ez a művelet hello Azure-portálon vagy a Windows PowerShell StorSimple.
2. Ismételje meg a fenti lépés tooshut hello aktív vezérlő le hello.
3. Kell most megnézzük hello vissza vezérlősík hello eszköz. Miután hello két vezérlő teljesen állnak le, hello állapot LED mindkét hello tartományvezérlőn kell kell villogó piros. Ha tooturn hello eszköz ki teljesen jelenleg, tükrözés hello power Power-és hűtési modulok (PCMs) vált toohello OFF pozícióját. Ez kapcsolja ki hello eszköz.

## <a name="reset-hello-device-toofactory-default-settings"></a>Hello eszköz toofactory alapértelmezett beállításainak alaphelyzetbe állítása

> [!IMPORTANT]
> Ha tooreset toofactory alapértelmezett beállításait, forduljon a Microsoft Support. Az alábbiakban hello eljárás csak a Microsoft Support párhuzamosan használható.

Ez az eljárás ismerteti, hogyan tooreset a Microsoft Azure StorSimple eszköz toofactory alapértelmezett beállítások a StorSimple Windows PowerShell használatával.
Gyári beállítások visszaállításával a eltávolítja minden adatot és beállítást hello fürtözési alapértelmezés szerint.

Hajtsa végre a következő lépéseket tooreset hello a Microsoft Azure StorSimple eszköz toofactory alapértelmezett beállítások:

### <a name="tooreset-hello-device-toodefault-settings-in-windows-powershell-for-storsimple"></a>tooreset hello eszköz toodefault beállításait a Windows PowerShell StorSimple
1. Hello eszközt a soros konzolon keresztül. Ellenőrizze, hogy-e csatlakoztatott toohello hello szalagcím üzenet tooensure **aktív** vezérlő.
2. Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.
3. Hello parancssorba írja be a következő parancs tooreset hello fürtözési, minden adatokat, a metaadatok és a tartományvezérlő-beállítások eltávolítása hello:
   
    `Reset-HcsFactoryDefault`
   
    tooinstead alaphelyzetbe állítani egy tartományvezérlő, használja a hello [alaphelyzetbe állítása-HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) hello parancsmagot `-scope` paraméter.)
   
    hello rendszer többször újraindul. Ön értesítést fog kapni hello alaphelyzetbe állítása sikeresen befejeződött. Attól függően, hogy hello rendszermodell 45-60 perces 8100-eszköz és a 60-90 percig, amíg egy 8600 toofinish ezt a folyamatot is tarthat.
   
## <a name="questions-and-answers-about-managing-device-controllers"></a>Kérdések és válaszok eszközvezérlők kezelése
Ez a szakasz azt kell foglalja össze néhány gyakori kérdés hello kezelése StorSimple eszközvezérlők tekintetében.

**K.** Mi történik, ha mindkét hello, tartományvezérlői meg az eszköz megfelelő és bekapcsolva és I leállítása vagy újraindítása hello aktív vezérlő?

**V.** Ha mindkét hello tartományvezérlők az eszközén kifogástalan, és van kapcsolva, akkor megerősítést kér. Ki is:

* **Indítsa újra az aktív vezérlő hello** – értesítés jelenik meg, hogy az aktív vezérlőhöz újraindítása miatt hello eszköz toofail toohello passzív vezérlő fölé. hello vezérlő indul újra.
* **Állítsa le az aktív vezérlőhöz** – értesítés jelenik meg, hogy az aktív vezérlőhöz leállítása a leállás eredményez. Meg kell hello eszköz tooturn hello tartományvezérlőn toopush hello főkapcsoló is.

**K.** Mi történik, ha hello passzív vezérlő meg az eszköz nem érhető el, és be van kapcsolva a kikapcsolva, és indítsa újra, vagy állítsa le a hello aktív vezérlő?

**V.** Ha az eszközön a hello passzív vezérlő ki van kapcsolva vagy nem érhető el, és úgy dönt, hogy:

* **Indítsa újra az aktív vezérlő hello** – értesítés jelenik meg, hogy a hello szolgáltatás ideiglenes szüneteltetése hello művelet folytatása eredményez, és megerősítést kér.
* **Állítsa le az aktív vezérlőhöz** – értesítés jelenik meg, hogy folyamatos hello művelet eredménye állásidő. Meg kell hello eszközön legalább az egyik tartományvezérlők tooturn toopush hello főkapcsoló is. Kell megerősítést kérni.

**K.** Ha nem hello tartományvezérlő újraindítása vagy leállítása sikertelen tooprogress?

**V.** A vezérlő leállítása és újraindítása sikertelen lehet, ha:

* Egy eszköz frissítése folyamatban van.
* A tartományvezérlő újraindítása már folyamatban van.
* Egy tartományvezérlő leállítás már folyamatban van.

**K.** Hogyan meg kitalálja, hogy ha egy tartományvezérlő újraindult, vagy állítsa le?

**V.** Ellenőrizheti, hogy a vezérlő panelen hello tartományvezérlő állapotát. hello tartományvezérlő állapotát fog jelzi, hogy a vezérlő hello folyamatának újraindítása és leállítása. Emellett hello **riasztások** panelre az információs riasztások tartalmazhat, ha a hello vezérlő újraindult, vagy állítsa le. hello vezérlő újraindítás és leállítás műveletek hello valók naplókat is rögzíti. Valók naplók kapcsolatos további információkért lépjen túl[hello tevékenységi naplóit megtekintése](storsimple-8000-service-dashboard.md#view-the-activity-logs).

**K.** Nincs semmilyen hatása toohello i/o vezérlő feladatátvétel miatt?

**V.** TCP-kapcsolatok hello kezdeményezők és az aktív vezérlő között vezérlő feladatátvétel miatt vissza lesz állítva, de hozni, ha a passzív vezérlő hello azt feltételezi, hogy a művelet. Valószínűleg i/o-tevékenységben kezdeményezők és hello eszköz között (kevesebb, mint 30 másodperc) ideiglenes szünetet hello Ez a művelet során.

**K.** Hogyan visszatérési meg a vezérlő tooservice, leállítása és eltávolítása után?

**V.** a vezérlő tooservice tooreturn, kell behelyezi hello váz leírtak [cserélje le a StorSimple eszköz vezérlő modul](storsimple-8000-controller-replacement.md).

## <a name="next-steps"></a>Következő lépések
* Ha problémába ütközik a StorSimple eszköz tartományvezérlőket ebben az oktatóanyagban szereplő hello eljárások használatával nem oldható fel a [forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md).
* bővebben hello StorSimple Device Manager szolgáltatást használó toolearn lépjen túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

