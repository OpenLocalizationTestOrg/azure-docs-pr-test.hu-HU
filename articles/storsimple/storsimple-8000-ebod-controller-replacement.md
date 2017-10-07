---
title: "a StorSimple 8600 EBOD vezérlő aaaReplace |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooremove, és cserélje le a StorSimple 8600 eszközön legalább az egyik EBOD tartományvezérlők."
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
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: 8343ed6f48ae97fc9204452f85e1936bfb1d6919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Cserélje le az EBOD vezérlőhöz a StorSimple eszköz

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag azt ismerteti, hogyan tooreplace egy hibás EBOD vezérlő modul a Microsoft Azure StorSimple eszközön. egy EBOD vezérlő modul tooreplace, kell:

* Hello hibás EBOD tartományvezérlő eltávolítása
* Egy új EBOD controller telepítése

Vegye figyelembe a következő információ megkezdése előtt hello:

* Üres EBOD modulok minden nem használt tárolóhelyek kell beszúrni. hello ház nem cool megfelelően, ha a tárhely marad nyitva.
* hello EBOD tartományvezérlő közbeni-cserélhető és eltávolítani vagy lecserélni. Ne távolítsa el egy hibás modul, amíg nem helyettesíti. Hello cseréjét kezdeményezésekor 10 percen belül kell befejezése.

> [!IMPORTANT]
> Tooremove megkísérlése előtt vagy StorSimple összetevők közül bármelyik lecseréléséhez győződjön meg arról, hogy tekintse át a hello [biztonsági ikon egyezmények](storsimple-safety.md#safety-icon-conventions) és egyéb [biztonsági óvintézkedéseket](storsimple-safety.md).

## <a name="remove-an-ebod-controller"></a>Távolítsa el az EBOD vezérlőhöz
Nem sikerült a StorSimple eszköz EBOD vezérlő moduljának hello behelyettesíteni, előtt győződjön meg arról, hogy hello más EBOD vezérlő modul az aktív és futó. hello alábbi eljárást és táblázatot azt ismertetik, hogyan tooremove hello EBOD vezérlő modul.

#### <a name="tooremove-an-ebod-module"></a>tooremove EBOD modul létrehozása
1. Nyissa meg hello Azure-portálon.
2. Nyissa meg tooyour eszközt, és keresse meg a túl**beállítások** > **hardver állapotának**, és ellenőrizze a hello LED hello állapotának hello aktív EBOD vezérlő modul zöld és hello LED hello nem sikerült az érték EBOD vezérlő modul az piros.
3. Keresse meg a sikertelen hello EBOD vezérlő modul: hello hello eszköz oldalán.
4. Távolítsa el a hello kábelek, mielőtt kilépteti a hello EBOD modul hello rendszerből hello EBOD vezérlő modul toohello vezérlő csatlakozó.
5. Jegyezze fel a pontos SAS port hello hello EBOD vezérlő modul, de a csatlakoztatott toohello vezérlő. Szükséges toorestore hello rendszerkonfiguráció toothis hello EBOD modul cseréje után lesz.
   
   > [!NOTE]
   > Általában ez lesz a Port, amely van címkézve **működteti** a következő diagram hello.
   
    ![Csatlakozópanel a EBOD vezérlő](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     **1. ábra** vissza a EBOD modul
   
   | Címke | Leírás |
   |:--- |:--- |
   | 1 |Tartalék LED |
   | 2 |Energiagazdálkodási LED |
   | 3 |SAS-összekötők |
   | 4 |SAS LED |
   | 5 |Csak a gyári használatra soros portokhoz |
   | 6 |Port (a gazdagép) |
   | 7 |B port (gazdagép kimenő) |
   | 8 |Port C (csak a gyári általi használatra) |

## <a name="install-a-new-ebod-controller"></a>Egy új EBOD controller telepítése
hello alábbi eljárást és táblázatot azt ismertetik, hogyan tooinstall egy EBOD vezérlő modulja a StorSimple eszközt.

#### <a name="tooinstall-an-ebod-controller"></a>az EBOD vezérlőhöz tooinstall
1. Ellenőrizze a hello EBOD eszköz sérülés, különösen a toohello felület összekötő. Ne telepítse hello új EBOD vezérlő, ha bármely elgörbülve.
2. A hello zárolás van életben a hello nyissa meg a pozíció, amíg hello zárolás van életben megszólítása hello ház hello modult menüpontban húzással állíthatja be.
   
    ![EBOD controller telepítése](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    **2. ábra** telepítése hello EBOD vezérlő modul
3. Bezárás hello zárolás. Egy kattintással kell hall, mivel hello zárolás kapcsolatba lép.
   
    ![EBOD zárolás feloldása](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    **3. ábra** hello EBOD modul zárolás bezárása
4. Csatlakozzon újra hello kábelek. A hello pontos konfigurációja hello cseréje előtt voltak. Tekintse meg a következő diagram hello és részletei tábla tooconnect hello kábelek.
   
    ![Az energiagazdálkodási 4U eszköz bekábelezése](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    **4. ábra**. Újracsatlakozás a kábelek.
   
   | Címke | Leírás |
   |:--- |:--- |
   | 1 |Elsődleges ház |
   | 2 |PCM 0 |
   | 3 |PCM 1 |
   | 4 |A vezérlő 0 |
   | 5 |1. vezérlő |
   | 6 |EBOD vezérlő 0 |
   | 7 |1. EBOD vezérlő |
   | 8 |EBOD ház |
   | 9 |Áramelosztó egységekből |

## <a name="next-steps"></a>Következő lépések
További információ [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).

