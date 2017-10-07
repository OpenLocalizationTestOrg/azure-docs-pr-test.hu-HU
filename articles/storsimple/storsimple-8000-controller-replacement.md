---
title: "aaaReplace a StorSimple 8000 sorozatú eszköz vezérlő |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooremove, és cserélje le a StorSimple 8000 series eszközön legalább az egyik vezérlő modulok."
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
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 76d42529da42dff2a214fb840a52392a7f1a97ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Cserélje le a StorSimple eszköz vezérlő modul
## <a name="overview"></a>Áttekintés
Ez az oktatóanyag azt ismerteti, hogyan tooremove, és cserélje le a StorSimple eszköz egyik vagy mindkét vezérlő modulokat. Emellett ismerteti a hello az alapul szolgáló hello egyszeres és kettős vezérlő cseréjekor logikát.

> [!NOTE]
> Előzetes tooperforming vezérlő helyettesíti, ajánlott mindig a vezérlő belső vezérlőprogramjának toohello legújabb verzióra frissíteni.
> 
> tooprevent tooyour StorSimple eszköz kárt, ne vegye ki hello vezérlő amíg hello LED jelennek meg a következő hello:
> 
> * Az összes fény OFF.
> * LED 3 ![zöld pipa ikon](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), és ![piros kereszt ikon](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) villogó vannak, és LED 0 és LED 7 **ON**.


a következő táblázat hello támogatott hello vezérlő cseréjekor jeleníti meg.

| Eset | Csere forgatókönyv | Az eljárás alkalmazható |
|:--- |:--- |:--- |
| 1 |Egy tartományvezérlő sikertelen állapotban van, hello más vezérlő kifogástalan és aktív. |[Egyetlen tartományvezérlő helyettesítő](#replace-a-single-controller), ismerteti, amelyek hello [egyetlen tartományvezérlő helyettesíti mögötti logika](#single-controller-replacement-logic), valamint hello [helyettesítő lépéseket](#single-controller-replacement-steps). |
| 2 |Mindkét hello tartományvezérlők sikertelen volt, és helyettesítő igényel. hello váz, a lemezek és a lemez ház nincs kifogástalan. |[Kettős vezérlő helyettesítő](#replace-both-controllers), hello ismerteti, amelyek [kettős vezérlő helyettesíti mögötti logika](#dual-controller-replacement-logic), valamint hello [helyettesítő lépéseket](#dual-controller-replacement-steps). |
| 3 |A tartományvezérlők hello ugyanarra az eszközre, vagy a különböző eszközök van cserélve. hello váz, a lemezek és a lemez ház nincs kifogástalan. |Egy tárolóhely eltérés figyelmeztető üzenet jelenik meg. |
| 4 |Egy tartományvezérlő hiányzik, és egyéb vezérlő sikertelen hello. |[Kettős vezérlő helyettesítő](#replace-both-controllers), hello ismerteti, amelyek [kettős vezérlő helyettesíti mögötti logika](#dual-controller-replacement-logic), valamint hello [helyettesítő lépéseket](#dual-controller-replacement-steps). |
| 5 |Legalább az egyik tartományvezérlők nem járt sikerrel. Hello eszköz hello soros konzol vagy a Windows PowerShell távoli eljáráshívás keresztül nem férhet hozzá. |[Forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md) manuális vezérlő helyettesítő eljárás. |
| 6 |Hello, tartományvezérlői verziónál eltérő, amelynek oka a következő lehet:<ul><li>Tartományvezérlők különböző szoftver verziója szükséges.</li><li>Tartományvezérlők különböző belső vezérlőprogram verziója szükséges.</li></ul> |Hello, tartományvezérlői szoftver verziója nem egyezik, ha hello helyettesítő logika, amely észleli, és frissítések hello hello helyettesítő vezérlőn szoftverének verziójával.<br><br>Ha hello vezérlő belső vezérlőprogramjának következő verziójával különböző és hello régi belsővezérlőprogram-verziónként **nem** automatikusan frissíthető, egy figyelmeztetés fog megjelenni hello Azure-portálon. Kell a frissítések keresése és telepítése hello belső vezérlőprogram-frissítésekre.</br></br>Ha hello vezérlő belső vezérlőprogramjának következő verziójával eltérőek, és automatikusan frissíthető hello régi belső vezérlőprogram-verziója, hello vezérlő helyettesítő logika észleli ezt, és hello vezérlő elindulása után hello belső vezérlőprogram automatikusan frissül. |

A vezérlő modul tooremove akkor kell, ha sikertelen volt. Legalább az egyik hello vezérlő modulok sikertelen lehet, ami eredményezhet egyetlen tartományvezérlő helyett vagy kettős vezérlő cserélni. Csere eljárások és azok mögötti hello logika: hello következő:

* [Egyetlen vezérlő cseréje](#replace-a-single-controller)
* [Cserélje le mindkét tartományvezérlők](#replace-both-controllers)
* [Távolítsa el a tartományvezérlő](#remove-a-controller)
* [A vezérlő beszúrása](#insert-a-controller)
* [Azonosítsa hello aktív az eszközön](#identify-the-active-controller-on-your-device)

> [!IMPORTANT]
> Mielőtt eltávolítása és cseréje egy tartományvezérlő, tekintse át a hello biztonsági adatokat a [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).
> 
> 

## <a name="replace-a-single-controller"></a>Cserélje le a csak egy vezérlő
Hello két vezérlőket hello Microsoft Azure StorSimple eszközön nem sikerült, hibás vagy hiányzik, úgy kell tooreplace csak egy vezérlő.

### <a name="single-controller-replacement-logic"></a>Egyetlen tartományvezérlő helyettesítő logika
Egyetlen tartományvezérlő helyettesíti el kell távolítani hello sikertelen vezérlő. (hello hello eszköz többi tartományvezérlőre az aktív vezérlő hello.) Hello helyettesítő tartományvezérlő beszúrásakor hello következő műveletek történnek meg:

1. hello helyettesítő tartományvezérlő azonnal elindítja a hello StorSimple eszköz kommunikál.
2. Hello virtuális merevlemez (VHD) a hello aktív vezérlő pillanatképe hello helyettesítő vezérlőn másolódik.
3. hello pillanatkép módosítását, hogy hello helyettesítő tartományvezérlő indításakor a virtuális merevlemezről felismerhető lesz készenléti tartományvezérlőként.
4. Hello módosítások be nem fejeződik, hello helyettesítő tartományvezérlő megkezdi hello készenléti tartományvezérlőjeként.
5. Ha mindkét hello tartományvezérlők futnak, hello fürtön online állapotba kerül.

### <a name="single-controller-replacement-steps"></a>Egyetlen tartományvezérlő helyettesítő lépései
Hajtsa végre a következő lépéseket, ha a Microsoft Azure StorSimple eszköz hello tartományvezérlőinek egyike meghibásodik hello. (hello más vezérlő aktív és futtatni kell. Ha mindkét tartományvezérlők sikertelen, vagy hibás működését, nyissa meg túl[kettős vezérlő helyettesítő lépéseket](#dual-controller-replacement-steps).)

> [!NOTE]
> Ez hello vezérlő toorestart 30 – 45 percet vesz igénybe, és teljesen helyre hello egyetlen tartományvezérlő helyettesítő eljárás. hello teljes hello teljes eljárást, beleértve a hello kábelek csatolása ideje körülbelül 2 óra.


#### <a name="tooremove-a-single-failed-controller-module"></a>tooremove egyetlen sikertelen vezérlő modul
1. Hello Azure portál, lépjen toohello StorSimple Device Manager szolgáltatást, kattintson **eszközök**, majd kattintson a megjeleníteni kívánt toomonitor hello eszköz hello nevét.
2. Nyissa meg túl**figyelő > hardver állapotának**. vagy a vezérlő 0, vagy a vezérlő 1 hello állapota kell piros, ami azt jelenti, hogy hiba.
   
   > [!NOTE]
   > hello sikertelen tartományvezérlőre, egyetlen tartományvezérlő helyettesíti a készenléti vezérlő.
   
3. 1. ábra és a következő tábla toolocate hello sikertelen vezérlő modul hello használata.
   
    ![Az eszköz elsődleges ház modulok Csatlakozópanel](./media/storsimple-controller-replacement/IC740994.png)
   
    **1. ábra** vissza a StorSimple eszköz
   
   | Címke | Leírás |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |A vezérlő 0 |
   | 4 |1. vezérlő |
4. Hello sikertelen tartományvezérlőre távolítsa el minden hello csatlakoztatott hálózati kábel hello adatok portok. Ha olyan 8600 modellt használ, eltávolítását SAS kábelek hello vezérlő toohello EBOD vezérlő csatlakozzon, amely hello.
5. Hello kövesse [távolítsa el a tartományvezérlő](#remove-a-controller) tooremove hello vezérlő nem sikerült.
6. Telepítés hello gyári csere a ugyanaz a tárolóhely mely hello sikertelen vezérlőből hello el lett távolítva. Ez elindítja a hello egyetlen tartományvezérlő helyettesítő logikát. További információkért lásd: [egyetlen tartományvezérlő helyettesítő logika](#single-controller-replacement-logic).
7. Hello egyetlen tartományvezérlő helyettesítő logika hello háttérben megfelelően, amíg újra hello kábelek. Minden hello kábel pontosan hello gondot tooconnect azonos módon, hogy volt csatlakoztatva hello cseréje előtt igénybe vehet.
8. Hello vezérlő újraindítását követően ellenőrizze hello **tartományvezérlő állapotát** és hello **állapot fürt** hello Azure portál, amely hello, tartományvezérlői tooverify hátsó tooa kifogástalan állapotban és a melegtartalék módban van.

> [!NOTE]
> Hello soros konzolon keresztül hello eszközök figyel, amíg hello vezérlő helyreállítja hello helyettesítő eljárás lásd: Előfordulhat, hogy több újraindítást. Hello soros konzol menü megjelenésekor, akkor tudja, hogy a hello csere befejeződött-e. Ha hello menü nem jelenik meg a kiindulási hello vezérlő helyettesítő két órán belül, [forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md).
>
> Update 4-től kezdődően parancsmaggal teheti meg is hello `Get-HCSControllerReplacementStatus` hello Windows PowerShell felületén hello eszköz toomonitor hello állapotáról hello, tartományvezérlői cseréjét.
> 

## <a name="replace-both-controllers"></a>Cserélje le mindkét tartományvezérlők
Ha mindkét hello Microsoft Azure StorSimple eszközön nem sikerült, amelyek hibásan működik, vagy hiányzik, meg kell tooreplace mindkét tartományvezérlők. 

### <a name="dual-controller-replacement-logic"></a>Kettős vezérlő helyettesítő logika
Kettős vezérlő helyettesíti először távolítsa el a mindkét sikertelen tartományvezérlőket, és helyezze be cserékhez. Hello két helyettesítő vezérlők szúrja be, amikor hello következő műveletek történnek meg:

1. hello helyettesítő tartományvezérlő adatszalagot a 0 hello következőket ellenőrzi:
   
   1. Akkor használja hello belső vezérlőprogram és a szoftver aktuális verziója?
   2. Ez hello fürt része?
   3. Hello társ tartományvezérlő fut, és ez a fürtözött?
      
      Ha ezek a feltételek egyike teljesül, a hello vezérlő keresi hello legújabb a napi biztonsági mentéshez (hello található **nonDOMstorage** S meghajtón). hello vezérlő hello VHD legújabb pillanatképe hello hello biztonsági mentés másolja át.
2. hello controller adatszalagot a 0 a hello pillanatkép tooimage magát.
3. Eközben hello vezérlő tárolóhelye 1 megvárja-e a vezérlő 0 toocomplete hello lemezképkészítés és a start.
4. Vezérlő 0 elindulása után a vezérlő 1 észleli 0, a tartományvezérlő által létrehozott hello fürt, amely elindítja a hello egyetlen tartományvezérlő helyettesítő logika. További információkért lásd: [egyetlen tartományvezérlő helyettesítő logika](#single-controller-replacement-logic).
5. Ezután mindkét tartományvezérlők fog futni, és hello fürtön online állapotba kerül.

> [!IMPORTANT]
> Kettős vezérlő helyettesíti, következő hello StorSimple eszköz konfigurálása után alapvető fontosságú, hogy szánjon kézi hello eszköz biztonsági mentését. Napi eszköz konfigurációs biztonsági másolatok addig nem által kiváltott, 24 óra eltelte után. Együttműködve [Microsoft Support](storsimple-8000-contact-microsoft-support.md) toomake az eszköz manuális biztonsági mentés.


### <a name="dual-controller-replacement-steps"></a>Kettős vezérlő helyettesítő lépései
Ez a munkafolyamat, ha mind a Microsoft Azure StorSimple eszköz hello tartományvezérlőinek sikertelen volt. Ez akkor fordulhat esetén egy adatközpontban, amelyben hello hűtési rendszer leáll, és emiatt mindkét hello tartományvezérlők sikertelen rövid időn belül. Attól függően e hello StorSimple eszköz ki van kapcsolva vagy, és hogy egy 8600 használ, vagy egy 8100 modell, lépéseket különböző szabálykészleteket szükséges.

> [!IMPORTANT]
> Ez hello vezérlő toorestart too1 óra 45 percig tarthat, és teljesen helyre kettős vezérlő helyettesítő eljárásból. hello teljes hello teljes eljárást, beleértve a hello kábelek csatolása ideje körülbelül 2,5 óra.

#### <a name="tooreplace-both-controller-modules"></a>tooreplace mindkét vezérlő modulok
1. Ha hello eszköz ki van kapcsolva, kihagyhatja ezt a lépést, és folytassa a következő lépésre toohello. Ha hello eszköz be van kapcsolva, kapcsolja ki a hello eszköz.
   
   1. Ha olyan 8600 modellt használ, kapcsolja ki a hello elsődleges ház első, és kapcsolja ki a hello EBOD ház.
   2. Várjon, amíg az eszköz hello teljesen leállt. Minden hello LED hátsó hello eszköz hello az ki lesz.
2. Távolítsa el az összes csatlakoztatott toohello adatok portok hello hálózati kábeleket. Ha olyan 8600 modellt használ, eltávolítását SAS kábelek hello elsődleges ház toohello EBOD ház csatlakozzon, amely hello.
3. Mindkét tartományvezérlők eltávolítása hello StorSimple eszközt. További információkért lásd: [távolítsa el a tartományvezérlő](#remove-a-controller).
4. Először beszúrása hello gyári helyettesíti a vezérlő 0, és helyezze a vezérlő 1. További információkért lásd: [vezérlő beszúrási](#insert-a-controller). Ez elindítja a hello kettős vezérlő helyettesítő logikát. További információkért lásd: [kettős vezérlő helyettesítő logika](#dual-controller-replacement-logic).
5. Hello vezérlő helyettesítő logika hello háttérben megfelelően, amíg újra hello kábelek. Minden hello kábel pontosan hello gondot tooconnect azonos módon, hogy volt csatlakoztatva hello cseréje előtt igénybe vehet. Lásd: hello részletes utasítások a hello kábel helyezheti modelljét az eszköz szakasza [a StorSimple 8100 eszköz telepítése](storsimple-8100-hardware-installation.md) vagy [a StorSimple 8600 eszköz telepítése](storsimple-8600-hardware-installation.md).
6. Hello StorSimple eszközön beállítás bekapcsolását. Ha olyan 8600 modellt használ:
   
   1. Győződjön meg arról, hogy hello EBOD ház első be van kapcsolva.
   2. Várjon, amíg hello EBOD ház fut-e.
   3. Kapcsolja be a hello elsődleges ház.
   4. Miután hello első tartományvezérlő újraindul, és megfelelő állapotban van, hello rendszer fog futni.
      
      > [!NOTE]
      > Hello soros konzolon keresztül hello eszközök figyel, amíg hello vezérlő helyreállítja hello helyettesítő eljárás lásd: Előfordulhat, hogy több újraindítást. Amikor hello soros konzol menü megjelenik, akkor tudja, hogy a hello csere befejeződött-e. Ha hello menü nem jelenik meg a kiindulási hello vezérlő helyettesítő 2,5 órán belül, [forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md).
     
## <a name="remove-a-controller"></a>Távolítsa el a tartományvezérlő
A StorSimple eszköz eljárás tooremove hibás vezérlő modul követően hello használja.

> [!NOTE]
> a következő cikkben szereplő ábrákat hello vannak a vezérlő 0. A vezérlő 1 Ezek akkor állítható vissza korábbi állapotba.


#### <a name="tooremove-a-controller-module"></a>a vezérlő modul tooremove
1. A görgetőgomb és mutatóujj között hello modul zárolás megfogható.
2. Óvatosan nyomja össze a görgetőgomb és mutatóujj együtt toorelease hello vezérlő zárolás.
   
    ![A tartományvezérlő zárolás feloldása](./media/storsimple-controller-replacement/IC741047.png)
   
    **2. ábra** felszabadításával vezérlő zárolás
3. Hello zárolás használja tartományvezérlőként leíró tooslide hello hello váz kívül.
   
    ![A késleltetett vezérlő váz kívül](./media/storsimple-controller-replacement/IC741048.png)
   
    **3. ábra** csúszó hello vezérlő hello váz kívül

## <a name="insert-a-controller"></a>A vezérlő beszúrása
Hibás a modul a StorSimple eszköz történő eltávolítása után a következő eljárás tooinstall gyári által megadott tartományvezérlő modul hello használata.

#### <a name="tooinstall-a-controller-module"></a>a vezérlő modul tooinstall
1. Ha bármely sérülést toohello felület összekötők toosee ellenőrzése Ha bármelyik hello összekötő PIN-kód sérült vagy hajlított ne telepítse hello modul.
2. Teljes felszabadul diák hello vezérlő modult hello váz hello zárolás közben.
   
    ![A váz mozgó vezérlő](./media/storsimple-controller-replacement/IC741053.png)
   
    **4. ábra** csúszó vezérlő hello váz be
3. Hello vezérlő modulra szúrja be először a bezárása közben folyamatos toopush hello vezérlő modul hello zárolás hello váz be. hello zárolás vesz részt tooguide hello vezérlő helyére.
   
    ![A tartományvezérlő zárolás bezárása](./media/storsimple-controller-replacement/IC741054.png)
   
    **5. ábra** hello vezérlő zárolás bezárása
4. Amikor hello zárolás dokkolása helyen végzett. Hello **OK** LED most kell lennie.
   
   > [!NOTE]
   > Hello, tartományvezérlői és hello LED tooactivate too5 percig is eltarthat.
  
5. tooverify, hogy hello helyettesítő sikeres, a hello Azure-portálon lépjen tooyour eszköz, és keresse meg a túl**figyelő** > **hardver állapotának**, és győződjön meg arról, hogy mindkét vezérlő 0 és 1. vezérlő rendszer kifogástalan (állapota zöld színnel).

## <a name="identify-hello-active-controller-on-your-device"></a>Azonosítsa hello aktív az eszközön
Vannak olyan helyzetek, például az első eszköz regisztrációs vagy vezérlő cserélni, igénylő toolocate hello aktív vezérlő a StorSimple eszközön. hello aktív vezérlő összes hello belső vezérlőprogram és hálózatkezelési lemezműveletekért dolgozza fel. A következő módszerek tooidentify hello aktív vezérlő hello bármelyikét használhatja:

* [Hello Azure portál tooidentify hello aktív vezérlővel használatával](#use-the-azure-portal-to-identify-the-active-controller)
* [A Windows PowerShell használata a StorSimple tooidentify hello aktív vezérlő](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)
* [Ellenőrizze a hello fizikai eszköz tooidentify hello aktív vezérlő](#check-the-physical-device-to-identify-the-active-controller)

Ezek az eljárások leírását mellett.

### <a name="use-hello-azure-portal-tooidentify-hello-active-controller"></a>Hello Azure portál tooidentify hello aktív vezérlővel használatával
A hello Azure-portálon, keresse meg a tooyour eszközt, majd túl**figyelő** > **hardver állapotának**, görgessen toohello **tartományvezérlők** szakasz. Itt ellenőrizheti, melyik tartományvezérlő aktív.

![Azonosítsa aktív Azure-portálon](./media/storsimple-controller-replacement/IC752072.png)

**6. ábra** az Azure portál ábrázoló hello aktív vezérlő

### <a name="use-windows-powershell-for-storsimple-tooidentify-hello-active-controller"></a>A Windows PowerShell használata a StorSimple tooidentify hello aktív vezérlő
Amikor az eszköz hello soros konzolon keresztül fér hozzá, transzparens üzenet akkor jelenik meg. Transzparens üdvözlőüzenetére például hello modell, a nevét, a telepített szoftverek verziója és a elérésére hello tartományvezérlő állapotának alapvető eszköz-információkat tartalmaz. a következő kép hello szalagcím üzenet példáját mutatja be:

![Soros szalagcím üzenet](./media/storsimple-controller-replacement/IC741098.png)

**7. ábra** szalagcím üzenet megjelenítő vezérlő 0 aktív

Hello szalagcím üzenet toodetermine használhatja akár hello tartományvezérlő van aktív vagy passzív toois csatlakoztatva.

### <a name="check-hello-physical-device-tooidentify-hello-active-controller"></a>Ellenőrizze a hello fizikai eszköz tooidentify hello aktív vezérlő
tooidentify hello aktív vezérlő az eszközön, keresse meg a kék hello LED fent hátsó hello elsődleges ház hello hello DATA 5-portjához.

Ha a LED villogó van, hello vezérlő aktív és hello egyéb vezérlő készenléti üzemmódban. A következő diagram hello használja, és elősegítésére tábla.

![Eszköz elsődleges ház csatlakozópanel rendelkező adatportok](./media/storsimple-controller-replacement/IC741055.png)

**8. ábra** hátsó elsődleges szolgáltatással együtt adatok portok és figyelési LED

| Címke | Leírás |
|:--- |:--- |
| 1-6 |DATA 0 – 5 hálózati portok |
| 7 |Kék LED |

## <a name="next-steps"></a>Következő lépések
További információ [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).

