---
title: "a Microsoft Azure StorSimple virtuális tömb aaaInstall frissítések |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello StorSimple virtuális tömb webes felhasználói felület tooapply frissíti hello portál és a gyorsjavítások a módszerrel"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 9997a97b-9382-43ed-b56e-61369335c987
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7424abc7e46d4f08b4eae1194642b263f32c4318
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-updates-on-your-storsimple-virtual-array---azure-portal"></a>Telepítse a frissítéseket a StorSimple virtuális tömb – az Azure-portálon

## <a name="overview"></a>Áttekintés

Ez a cikk ismerteti a hello lépéseket szükséges tooinstall frissítések a StorSimple virtuális tömb hello Azure-portálon és hello helyi webes felhasználói felületen keresztül. Meg kell tooapply szoftver frissítéseket vagy gyorsjavításokat tookeep a StorSimple virtuális tömb naprakész. 

Ne feledje, hogy egy frissítés vagy gyorsjavítás telepítése az eszköz újraindul. Adott meg, hogy a StorSimple virtuális tömb hello egycsomópontos eszköz, folyamatban lévő összes i/o megszakad, és az eszköz leállást észlel. 

Egy frissítés alkalmazása előtt ajánlott végrehajtása hello kötetek vagy megosztások offline hello először meg, és ezután hello eszköz. Ez minimalizálja az adatvesztést lehetőségét.

> [!IMPORTANT]
> Ha futtatja a frissítési 0,1 vagy GA szoftververziók, hello helyi webes felhasználói felület tooinstall Update 0,3 hello gyorsjavítás metódust kell használnia. Ha futtatja a frissítési 0,2, ajánlott hello a klasszikus Azure portálon keresztül hello frissítések telepítése.
 

## <a name="use-hello-local-web-ui"></a>Hello helyi webes felhasználói felület használata

Két lépésből áll hello helyi webes felhasználói felület használata esetén:

* Hello frissítést vagy gyorsjavítást hello letöltése
* Hello frissítés vagy gyorsjavítás hello telepítése

### <a name="download-hello-update-or-hello-hotfix"></a>Hello frissítést vagy gyorsjavítást hello letöltése

Hajtsa végre a következő lépéseket toodownload hello szoftverfrissítést a Microsoft Update katalógus hello hello.

#### <a name="toodownload-hello-update-or-hello-hotfix"></a>toodownload hello frissítés vagy hello gyorsjavítás.

1. Indítsa el az Internet Explorert, és keresse meg a túl[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Ha az első alkalommal használja a Microsoft Update katalógus hello ezen a számítógépen, kattintson a **telepítése** amikor felszólító tooinstall hello bővítmény a Microsoft Update katalógusból.

3. A Microsoft Update katalógus hello hello a keresési mezőbe, írja be a hello Tudásbázis (KB) száma hello gyorsjavítás toodownload szeretné. Adja meg **3182061** frissítési 0,3, és kattintson a **keresési**.
   
    hello gyorsjavítás lista megjelenik, például **StorSimple virtuális tömb frissítés 0,3**.
   
    ![Keresés a katalógusban](./media/storsimple-virtual-array-install-update/download1.png)

4. Kattintson az **Add** (Hozzáadás) parancsra. hello frissítés hozzá legyen adva toohello kosár.

5. Kattintson a **Kosár megtekintése** gombra.

6. Kattintson a **Letöltés** gombra. Adja meg vagy **Tallózás** tooa helyi helyének hello tooappear tölti le. hello frissítések letöltése toohello megadott helyre, majd egy almappát azonos nevet hello frissítésként hello helyezi. hello mappába másolt tooa hálózati megosztásra, amely elérhető hello eszköz is lehet.

7. Nyitott hello mappába másolta, a Microsoft Update önálló csomagfájl kell megjelennie `WindowsTH-KB3011067-x64`. Ez a fájl használt tooinstall hello frissítést vagy gyorsjavítást.

### <a name="install-hello-update-or-hello-hotfix"></a>Hello frissítés vagy gyorsjavítás hello telepítése

Előzetes toohello frissítés vagy gyorsjavítás telepítési, győződjön meg arról, hogy hello frissítést vagy hello gyorsjavítás letöltése vagy a gazdagépen helyileg vagy egy hálózati megosztáson keresztül érhető el. 

A metódus tooinstall frissítések használatához GA futtató eszközök, vagy 0,1 szoftververziók frissítése. Ez az eljárás kisebb, mint 2 percet toocomplete vesz igénybe. Hajtsa végre a következő hello lépések tooinstall hello frissítést vagy gyorsjavítást.

#### <a name="tooinstall-hello-update-or-hello-hotfix"></a>tooinstall hello frissítés vagy hello gyorsjavítás.

1. Hello helyi webes felhasználói felületén, nyissa meg túl**karbantartási** > **szoftverfrissítés**.
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update/update1m.png)

2. A **frissítés fájl elérési útját**adja meg a fájlnevet hello hello frissítés vagy gyorsjavítás hello. Toohello frissítés vagy gyorsjavítás telepítési fájl is megkeresheti, ha egy hálózati megosztáson. Kattintson az **Alkalmaz** gombra.
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update/update2m.png)

3. A figyelmeztetés akkor jelenik meg. Ez a megadott van egy egycsomópontos eszköz után hello frissítés, hello eszköz újraindul, és nincs leállás. Kattintson a pipa ikonra hello.
   
   ![eszköz frissítése](./media/storsimple-virtual-array-install-update/update3m.png)

4. hello frissítés elindul. Hello eszköz sikeres frissítése után újraindul. hello helyi felhasználói felület mpelement nem érhető el ennek az időtartamnak.
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update/update5m.png)

5. Hello újraindítás befejezése után a következő lépés az toohello **bejelentkezés** lap. tooverify hello eszközhöz frissített, hello helyi webes felhasználói felületén, nyissa meg túl**karbantartási** > **szoftverfrissítés**. megjelenített hello szoftverének verziójával kell **10.0.0.0.0.10288.0** 0,3 frissítés.
   
   > [!NOTE]
   > A Microsoft hello helyi webes felhasználói felület némileg eltérő módon hello szoftververziók jelentést, és hello Azure-portálon. Például a helyi webes felhasználói felület jelentések hello **10.0.0.0.0.10288** és az Azure portál jelentések hello **10.0.10288.0** hello az azonos verzióját.
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update/update6m.png)

## <a name="use-hello-azure-portal"></a>Hello Azure portál használata

Frissítés 0,2 fut, azt javasoljuk, hogy telepítse a frissítéseket hello Azure-portálon keresztül. hello portál eljáráshoz szükséges hello felhasználói tooscan, töltse le és telepítse a hello frissítések. Ezzel az eljárással toocomplete körülbelül 7 percet vesz igénybe. Hajtsa végre a következő hello lépések tooinstall hello frissítést vagy gyorsjavítást.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal.md)]

Hello után egy telepítés befejeződött (a feladatok állapota a 100 %-os), nyissa meg tooyour StorSimple Device Manager szolgáltatást. Válassza ki **eszközök** majd válassza ki, és kattintson a kívánt eszközök csatlakoztatott toothis szolgáltatás hello listája tooupdate hello eszköz. A hello **beállítások** panelen, lépjen túl**kezelése** válassza ki azt **eszközfrissítésekhez**. megjelenített hello szoftverének verziójával kell **10.0.10288.0**.


## <a name="next-steps"></a>Következő lépések

További információ [felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).

