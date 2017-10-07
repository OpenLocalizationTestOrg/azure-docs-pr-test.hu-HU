---
title: "a StorSimple virtuális tömb 0,6 módosításának aaaInstall |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello StorSimple virtuális tömb webes felhasználói felület tooapply frissítések segítségével hello Azure portál és a gyorsjavítások módszer"
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
ms.date: 05/18/2017
ms.author: alkohli
ms.openlocfilehash: 2ccd1b5fc1957c35ebec35aa947d331b3ff05464
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-06-on-your-storsimple-virtual-array"></a>A StorSimple virtuális tömb 0,6 frissítés telepítése

## <a name="overview"></a>Áttekintés

Ez a cikk ismerteti a hello lépéseket szükséges tooinstall frissítés 0,6 a StorSimple virtuális tömb hello Azure-portálon és hello helyi webes felhasználói felületen keresztül. Hello szoftver frissítéseket vagy gyorsjavításokat tookeep alkalmazni a StorSimple virtuális tömb naprakész.

Egy frissítés alkalmazása előtt ajánlott végrehajtása hello kötetek vagy megosztások offline hello először meg, és ezután hello eszköz. Ez minimalizálja az adatvesztést lehetőségét. Miután hello kötetek vagy megosztások offline üzemmódban, is gondolja kézi hello eszköz biztonsági mentését.

> [!IMPORTANT]
> - Frissítés 0,6 megfelel-e túl**10.0.10293.0** szoftverének verziójával, az eszközön. Információ a what's new in a frissítés: túl[kibocsátási megjegyzései frissítés 0,6](storsimple-virtual-array-update-06-release-notes.md).
>
> - Ha a frissítés 0,2 futtatja, vagy később, akkor javasoljuk, hogy telepítenie hello frissítések hello Azure-portálon keresztül. Ha a frissítés 0,1 vagy GA szoftver verziója fut, hello gyorsjavítás metódus hello helyi webes felhasználói felület tooinstall frissítés 0,6 keresztül kell használnia.
>
> - Ne feledje, hogy egy frissítés vagy gyorsjavítás telepítése az eszköz újraindul. Adott meg, hogy a StorSimple virtuális tömb hello egycsomópontos eszköz, folyamatban lévő összes i/o megszakad, és az eszköz leállást észlel.

## <a name="use-hello-azure-portal"></a>Hello Azure portál használata

Update futtatása 0,2 és újabb verziók, azt javasoljuk, hogy telepítse a frissítéseket hello Azure-portálon keresztül. hello portál eljáráshoz szükséges hello felhasználói tooscan, töltse le és telepítse a hello frissítések. Ezzel az eljárással toocomplete körülbelül 7 percet vesz igénybe. Hajtsa végre a következő hello lépések tooinstall hello frissítést vagy gyorsjavítást.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Hello után egy telepítés befejeződött, nyissa meg tooyour StorSimple Device Manager szolgáltatást. Válassza ki **eszközök** majd válassza ki, és kattintson a frissített hello eszköz. Nyissa meg túl**beállítások > kezelés > Eszközfrissítésekhez**. megjelenített hello szoftverének verziójával kell **10.0.10293.0**.

## <a name="use-hello-local-web-ui"></a>Hello helyi webes felhasználói felület használata

Két lépésből áll hello helyi webes felhasználói felület használata esetén:

* Hello frissítést vagy gyorsjavítást hello letöltése
* Hello frissítés vagy gyorsjavítás hello telepítése

### <a name="download-hello-update-or-hello-hotfix"></a>Hello frissítést vagy gyorsjavítást hello letöltése

Hajtsa végre a következő lépéseket toodownload hello szoftverfrissítést a Microsoft Update katalógus hello hello.

#### <a name="toodownload-hello-update-or-hello-hotfix"></a>toodownload hello frissítés vagy hello gyorsjavítás.

1. Indítsa el az Internet Explorert, és keresse meg a túl[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Ha használ a Microsoft Update katalógus hello hello az első alkalommal ezen a számítógépen, kattintson a **telepítése** amikor felszólító tooinstall hello Microsoft Update katalógus bővítmény.

3. A Microsoft Update katalógus hello hello a keresési mezőbe, írja be a hello Tudásbázis (KB) száma hello gyorsjavítás toodownload szeretné. Adja meg **4023268** frissítési 0,6, és kattintson a **keresési**.
   
    hello gyorsjavítás lista megjelenik, például **StorSimple virtuális tömb frissítés 0,6**.
   
    ![Keresés a katalógusban](./media/storsimple-virtual-array-install-update-06/download1.png)

4. Kattintson a **Letöltés** gombra.

5. Öt fájlok toodownload kell megjelennie. Töltse le, minden egyes ezen fájlok tooa mappa. hello mappába másolt tooa hálózati megosztásra, amely elérhető hello eszköz is lehet.

6. Nyissa meg a hello mappát, ahol hello fájlok találhatók.
    ![Fájlok hello csomag](./media/storsimple-virtual-array-install-update-06/update06folder.png)

    Lásd:
    -  A Microsoft Update önálló csomagfájl `WindowsTH-KB3011067-x64`. Ez a fájl egy használt tooupdate hello eszköz szoftver.
    - Geneva figyelési ügynök csomagfájl `GenevaMonitoringAgentPackageInstaller`. A fájl használt tooupdate hello megfigyelési és diagnosztikai szolgáltatás (MDS) ügynök. Kattintson duplán a cab-fájl hello. A _.msi_ jelenik meg. Válassza hello fájlt, kattintson a jobb gombbal, majd **kibontása** hello fájl. Hello használata _.msi_ fájl tooupdate hello ügynök.

        ![Az MDS-ügynök frissítése fájl kibontása](./media/storsimple-virtual-array-install-update-06/extract-geneva-monitoring-agent-installer.png)

        > [!IMPORTANT]
        > Ha futtatja a StorSimple frissítés 0,5 (0.0.10293.0) nem kell tooupdate hello MDS ügynök.

    - Három fontos Windows biztonsági frissítéseket tartalmazó fájlokat `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, és `windows8.1-kb4019213-x64`.


### <a name="install-hello-update-or-hello-hotfix"></a>Hello frissítés vagy gyorsjavítás hello telepítése

Előzetes toohello frissítés vagy gyorsjavítás telepítési, győződjön meg arról, hogy hello frissítést vagy hello gyorsjavítás letöltése vagy a gazdagépen helyileg vagy egy hálózati megosztáson keresztül érhető el.

A metódus tooinstall frissítések használatához GA futtató eszközök, vagy 0,1 szoftververziók frissítése. Ez az eljárás körülbelül 12 perc toocomplete vesz igénybe. Hajtsa végre a következő hello lépések tooinstall hello frissítést vagy gyorsjavítást.

#### <a name="tooinstall-hello-update-or-hello-hotfix"></a>tooinstall hello frissítés vagy hello gyorsjavítás.

1. Hello helyi webes felhasználói felületén, nyissa meg túl**karbantartási** > **szoftverfrissítés**. Jegyezze fel az Ön által futtatott hello szoftverének verziójával. Ha futtatja **10.0.10290.0**, nem kell tooupdate hello MDS ügynök 6. lépésben.
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. A **frissítés fájl elérési útját**adja meg a fájlnevet hello hello frissítés vagy gyorsjavítás hello. Toohello frissítés vagy gyorsjavítás telepítési fájl is megkeresheti, ha egy hálózati megosztáson. Kattintson az **Alkalmaz** gombra.
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. A figyelmeztetés akkor jelenik meg. A megadott hello virtuális egy egycsomópontos eszköz hello frissítés, hello eszköz újraindul, és nincs leállás után. Kattintson a pipa ikonra hello.
   
   ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. hello frissítés elindul. Hello eszköz sikeres frissítése után újraindul. hello helyi felhasználói felület mpelement nem érhető el ennek az időtartamnak.
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. Hello újraindítás befejezése után a következő lépés az toohello **bejelentkezés** lap. tooverify hello eszközhöz frissített, hello helyi webes felhasználói felületén, nyissa meg túl**karbantartási** > **szoftverfrissítés**. megjelenített hello szoftverének verziójával kell **10.0.0.0.0.10293** 0,6 frissítés.
   
   > [!NOTE]
   > A Microsoft hello helyi webes felhasználói felület némileg eltérő módon hello szoftververziók jelentést, és hello Azure-portálon. Például a helyi webes felhasználói felület jelentések hello **10.0.0.0.0.10293** és az Azure portál jelentések hello **10.0.10293.0** hello az azonos verzióját.
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-06/update6m.png)

6. Kihagyhatja ezt a lépést, ha a StorSimple virtuális tömb frissítés 0,5 futtatása során (**10.0.10290.0**) a frissítés alkalmazása előtt. Feljegyezte hello szoftverének verziójával, az 1. lépésben tooupdate megkezdése előtt. Ha 0,5 frissítés futtatásakor, az MDS-ügynök már naprakész.

    Ha egy szoftver verziója előzetes tooUpdate 0,5 futtat, a hello következő lépés az Ön tooupdate hello MDS ügynök. A hello **szoftverfrissítés** lapon, nyissa meg toohello **frissítés fájl elérési útját** , és keresse meg a toohello `GenevaMonitoringAgentPackageInstaller.msi` fájlt. Ismételje meg a 2 – 4. Hello virtuális tömb újraindítása után jelentkezzen be hello helyi webes felhasználói felület.

7. 2 – 4 tooinstall hello Windows biztonsági javítások fájlokkal megismétli `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, és `windows8.1-kb4019213-x64`. hello virtuális tömb minden telepítése után újraindul, és a hello helyi webes felhasználói felület toosign kell.

## <a name="next-steps"></a>Következő lépések

További információ [felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).

