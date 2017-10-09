---
title: "aaaUse hello StorSimple Manager eszköz irányítópult |} Microsoft Docs"
description: "Hello StorSimple Manager szolgáltatás eszközök irányítópult ismerteti, hogyan toouse azt tooview tárolási metrikák és a csatlakoztatott kezdeményezők és a keresés hello sorozatszámát és IQN."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6c213969-a385-461f-b698-78ef5b8a79cc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e213fc0a081c21b9d6b408a3dd845cc93a31e250
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-device-dashboard-in-storsimple-manager-service"></a>A StorSimple Manager szolgáltatás hello eszköz irányítópult használata  

## <a name="overview"></a>Áttekintés
hello StorSimple Manager eszköz irányítópult lehetővé teszi az adott StorSimple eszköz információk áttekintését, ezzel szemben a toohello szolgáltatás irányítópultot, melyet a Microsoft Azure StorSimple megoldásban hello eszközeiről adatait jeleníti meg.

![Irányítópult-oldalon eszköz](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

az irányítópulton hello hello a következő információkat:

* **Diagram – terület** – hello megfelelő storage mérőszámainak hello diagramterületen hello irányítópult hello tetején látható. Ezen a diagramon hello teljes elsődleges tárterület (állomások tooyour eszköz által írt adatok mennyiségét hello) metrikáit megtekintheti és hello teljes felhőbeli tárhelyén, az eszköz által felhasznált, egy meghatározott időtartamra vonatkozóan.
  
     Ebben a környezetben *elsődleges tárolási* hello állomás által írt adatok teljes mennyiségének toohello hivatkozik, és a kötet típusa szerint sorolhatók: *elsődleges rétegzett tárolás* egyaránt helyben tárolt adatok és az adatok rétegzett toohello felhő; *elsődleges helyileg rögzített tárolási* csak helyben tárolja az adatokat tartalmazza. *Felhőbeli tárhely*, a hello ugyanakkor, a hello hello felhőben tárolt adatok teljes mennyiségének mérését. Ez magában foglalja a rétegzett adatok és a biztonsági mentéseket. Vegye figyelembe, hogy hello felhőben tárolt adatok deduplikációja és tömöríti, mivel elsődleges tárolási azt hello hello adatok előtt használt tároló deduplikált, és a tömörített. (Ezek két szám tooget hello tömörítési arány képet is összehasonlíthatja.) A mind az elsődleges és a felhőbeli tárhelyén, hello összegek konfigurált gyakoriságnak követési hello alapul. Például ha úgy dönt, hogy az egy hét gyakoriságát, majd hello diagram fog adatok megjelenítése minden nap az előző hét a hello.
  
     Hello diagram az alábbiak szerint állíthatja be:
  
  * felhőalapú tárolás felhasznált idő, jelölje be hello keresztül toosee hello mennyisége **FELHŐALAPÚ TÁROLÓT használja a** lehetőséget. toosee hello tárhelyet hello állomás, jelölje be hello írt **elsődleges RÉTEGZETT TÁROLÓT használja a** és **elsődleges HELYILEG rögzített TÁROLÓT használja a** beállítások. Az ábrán hello mindkét lehetőség van kiválasztva; ezért hello diagram ábrázolja a felhő- és tárolási elsődleges tárolási összegek. Vegye figyelembe, hogy bármely elsődleges tárolót használja a 2. frissítés előzetes tooinstalling hello által képviselt **elsődleges RÉTEGZETT TÁROLÓT használja a** sor.
  * Használjon hello legördülő menü hello diagram toospecify 1 hét, hónap 1, 3 hónapos vagy 1 év időszak hello jobb felső sarkában. Vegye figyelembe, hogy a legfelső szintű diagram hello frissülnek naponta csak egyszer, és ezért fogja tartalmazni előző napi összegek hello.
    
    További információkért lásd: [használata hello StorSimple Manager szolgáltatás toomonitor a StorSimple eszköz](storsimple-monitor-device.md).
* **Használati áttekintése** – hello a **használatának áttekintése** területen látható használt elsődleges tárolási hello mérete, kiépített, és a maximális tárolókapacitás hello az eszköz hello mennyiségét. A használati számok toohello maximális mennyisége elérhető tárterület összehasonlításával láthatja egy pillanat alatt Ha tooobtain további tárhely szükséges. Ne feledje, hogy ez az Áttekintés 15 percenként frissül, hello eltérő frissítési gyakoriság, előfordulhat, hogy különböző számának megjelenítése mint látható hello diagramterületen fenti, amelyet naponta frissít. További információkért lásd: [használata hello StorSimple Manager szolgáltatás toomonitor a StorSimple eszköz](storsimple-monitor-device.md).
* **Riasztások** – hello **riasztások** terület hello riasztások az eszköz áttekintését tartalmazza. Riasztások súlyosság szerint vannak csoportosítva, és a szám áll minden súlyossági szintű riasztások hello száma. Hello riasztás súlyossági hello a hatókörön belüli nézet megnyitása kattintva riasztások lapon tooshow csak hello riasztásokat az adott súlyossági szint ehhez az eszközhöz.
* **Feladatok** – hello **feladatok** terület azt mutatja be, hogy hello legutóbbi feladat tevékenység eredményeit. Ez biztosítsák, hello rendszer a várt módon működik, vagy megőrizheti azt jelzi, hogy kell-e tootake kiigazító intézkedéseket. toosee további tájékoztatás nemrég befejeződött, kattintson a **hello utolsó 24 órában feladatok sikeres**.
* Hello **gyors áttekintő** sarkában hello irányítópult hello területén nyújtanak hasznos információkat, például az eszköz típusa, sorozatszámát, állapot, leírás és kötetek száma.

Feladatátvétel konfigurálása is, majd csatlakoztatott kezdeményezők hello eszköz irányítópulton megtekintheti.

Ezen az oldalon végrehajtott hello gyakori feladatok a következők:

* Csatlakoztatott kezdeményezők megtekintése
* Hello eszközsorozatszámot keresése
* Hello eszköz cél IQN keresése

## <a name="view-connected-initiators"></a>Csatlakoztatott kezdeményezők megtekintése
Hello iSCSI-kezdeményezők, amelyek csatlakoztatott tooyour eszköz hello kattintva megtekintheti **nézet csatlakoztatott kezdeményezők** hello található hivatkozás **gyors áttekintő** az eszköz irányítópult területen. Ez a lap felsorolja táblázatos hello-kezdeményezők, amelyek sikeresen csatlakozott tooyour eszköz. Minden kezdeményezőnél látható:

* hello iSCSI minősített nevét (IQN) hello kezdeményező csatlakoztatva.
* hello hello hozzáférés-vezérlési rekordot (ACR), amely lehetővé teszi a csatlakoztatott kezdeményező neve.
* hello IP-címe hello kezdeményező csatlakoztatva.
* hello hálózati adapterek, hogy hello kezdeményező csatlakoztatott tooon azonban a tárolóeszköz. Ezek között lehet DATA 0 tooDATA 5.
* Az összes csatlakoztatott kezdeményező hello hello kötetek engedélyezett tooaccess toohello aktuális ACR konfigurációja alapján történik.

Ha tekintse meg a listában szereplő váratlan kezdeményezők vagy hello várt azokat nem látja, tekintse át a ACR konfigurációját. Legfeljebb 512 kezdeményezők csatlakoztatni tooyour eszközt.

## <a name="find-hello-device-serial-number"></a>Hello eszközsorozatszámot keresése
A Microsoft többutas I/O (MPIO) hello eszközön konfigurálásakor szükség lehet hello eszköz sorozatszámát. Hajtsa végre a következő lépéseket toofind hello eszközsorozatszámot hello.

#### <a name="toofind-hello-device-serial-number"></a>toofind hello sorozatszámát
1. Keresse meg a túl**eszközök** > **irányítópult**.
2. Hello hello irányítópult jobb oldali ablaktáblában található hello **gyors áttekintő** területen.
3. Görgessen lefelé, és keresse meg a hello sorozatszámát.

## <a name="find-hello-device-target-iqn"></a>Hello eszköz cél IQN keresése
Szükség lehet hello eszköz cél IQN hello Challenge Handshake Authentication Protocol (CHAP) a StorSimple eszköz konfigurálásakor. Hajtsa végre a következő lépéseket toofind hello eszköz cél IQN hello.

### <a name="toofind-hello-device-target-iqn"></a>toofind hello eszköz cél IQN
1. Keresse meg a túl**eszközök** > **irányítópult**.
2. Hello hello irányítópult jobb oldali ablaktáblában található hello **gyors áttekintő** területen.
3. Görgessen lefelé, és keresse meg a hello cél IQN.

## <a name="next-steps"></a>Következő lépések
* További tudnivalók hello [StorSimple Manager szolgáltatás irányítópultját](storsimple-service-dashboard.md).
* További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

