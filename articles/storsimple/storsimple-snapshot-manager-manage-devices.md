---
title: "a StorSimple Snapshot Manager aaaManage eszközök |} Microsoft Docs"
description: "Útmutatás a toouse StorSimple Snapshot Manager MMC beépülő modul tooconnect hello és a StorSimple eszközök kezeléséhez."
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 966ecbe3-a7fa-4752-825f-6694dd949946
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 7a2a2ca830e4ea6eb4b01f2542958df3871c1700
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-tooconnect-and-manage-storsimple-devices"></a>StorSimple Snapshot Manager tooconnect használja, és a StorSimple eszközök kezelése
## <a name="overview"></a>Áttekintés
A StorSimple Snapshot Manager hello is használhatja a csomópontok **hatókör** ablaktábla tooverify importált StorSimple eszközön tárolt adatok, és frissítse a csatlakoztatott tárolóeszközök. Emellett elemre hello **eszközök** csomópont, megtekintheti a csatlakoztatott eszközök és a megfelelő állapot adatai hello **eredmények** ablaktáblán.

![Csatlakoztatott eszközök](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**1. ábra: A StorSimple Snapshot Manager csatlakoztatott eszközön** 

Attól függően a **nézet** beállításokat, hello **eredmények** ablaktáblán látható minden eszközére vonatkozó információt a következő hello. (Nézet konfigurálásával kapcsolatos további információkért lépjen túl[Nézet menü](storsimple-use-snapshot-manager.md#view-menu).

| Eredmények oszlop | Leírás |
|:--- |:--- |
| Név |a klasszikus Azure portálon hello hello eszköz hello neve |
| Modell |hello eszköz hello modell száma |
| Verzió |hello telepített szoftvereket eszközön hello hello verziója |
| status |E hello eszköz rendelkezésre áll-e |
| Az utolsó szinkronizálás |Dátum és idő, amikor hello eszköz volt utoljára szinkronizálva |
| Sorozatszám |hello hello eszköz sorozatszámát |

Ha ezt a hello **eszközök** hello csomópontja **hatókör** ablaktáblán választhat hello a következő műveleteket:

* Adja hozzá, vagy cserélje le az eszköz
* Csatlakoztasson egy eszközt, és ellenőrizze a importálása
* Frissítse a csatlakoztatott eszközök

Ha hello **eszközök** csomópontot, és kattintson rá jobb gombbal egy eszköz neve a hello **eredmények** ablaktáblán választhat a következő műveletek hello:

* Egy eszköz hitelesítéséhez
* Eszköz részleteinek megtekintése
* Egy eszköz frissítése
* Egy eszköz konfigurálásának törlése
* A jelszó módosítása

> [!NOTE]
> Az összes ilyen műveletek is rendelkezésre állnak a hello **műveletek** ablaktáblán.


Ez az oktatóanyag azt ismerteti, hogyan toouse StorSimple Snapshot Manager tooconnect és eszközök kezeléséhez, és hajtsa végre a következő feladatok hello:

* Adja hozzá, vagy cserélje le az eszköz
* Csatlakoztasson egy eszközt, és ellenőrizze a importálása
* Frissítse a csatlakoztatott eszközök
* Egy eszköz hitelesítéséhez
* Eszköz részleteinek megtekintése
* Egy adott eszköz frissítése
* Egy eszköz konfigurálásának törlése
* Egy lejárt jelszó módosítása
* Cserélje le a hibás eszköz

> [!NOTE]
> Hello StorSimple Snapshot Manager felület használatával kapcsolatos általános információkért nyissa meg túl[StorSimple Snapshot Manager felhasználói felület](storsimple-use-snapshot-manager.md).


## <a name="add-or-replace-a-device"></a>Adja hozzá, vagy cserélje le az eszköz
Használja a következő eljárás tooadd hello, vagy cserélje le a StorSimple eszköz.

#### <a name="tooadd-or-replace-a-device"></a>tooadd vagy a név felülírandó eszköz
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblában kattintson a jobb gombbal hello **eszközök** csomópontra, majd **eszköz konfigurálása**. Hello **eszköz konfigurálása** párbeszédpanel jelenik meg.
   
    ![A StorSimple eszköz konfigurálása](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 
3. A hello **eszköz** legördülő mezőben, válassza hello hello vagy virtuális eszköz IP-címét. 
4. A hello **jelszó** , a típus hello StorSimple Snapshot Manager jelszavát, amelyet a klasszikus Azure portálon hello hello eszköz hozott létre. Kattintson az **OK** gombra. StorSimple Snapshot Manager hello eszköz azonosított keres. 
   
   * Hello eszköz érhető el, ha a StorSimple Snapshot Manager hozzáadja a kapcsolat.
   * Ha bármilyen okból hello eszköz nem érhető el, a StorSimple Snapshot Manager hibaüzenetet ad vissza. Kattintson a **OK** tooclose hello hibaüzenet, és kattintson a **Mégse** tooclose hello **eszköz konfigurálása** párbeszédpanel megnyitásához.

## <a name="connect-a-device-and-verify-imports"></a>Csatlakoztasson egy eszközt, és ellenőrizze a importálása
A következő eljárás tooconnect a StorSimple eszköz hello használja, és győződjön meg arról, hogy minden meglévő kötet csoportokhoz társított biztonsági másolatok lettek importálva.

#### <a name="tooconnect-a-device-and-verify-imports"></a>tooconnect egy eszközt, és ellenőrizze az importáláshoz
1. egy eszköz tooStorSimple Snapshot Manager tooconnect Hozzáadás hello utasításokat követve, vagy cserélje ki egy eszközt. Kapcsolódáskor tooa eszközt, az alábbiak szerint válaszol a StorSimple Snapshot Manager:
   
   * Ha bármilyen okból hello eszköz nem érhető el, a StorSimple Snapshot Manager hibaüzenetet ad vissza. 
   
   * Hello eszköz érhető el, ha a StorSimple Snapshot Manager hozzáadja a kapcsolat. Hello eszköz kiválasztásakor megjelenik hello **eredmények** ablaktáblán, és hello állapot mező jelzi, hogy hello eszköz **elérhető**. StorSimple Snapshot Manager importálja hello eszköz, a kötet-csoportokhoz, feltéve, hogy a kötet csoportok hello társított biztonsági másolatok. Biztonsági mentési házirendek nincsenek importálva. Kötet csoportok, amelyek nem rendelkeznek társított biztonsági másolatok nincsenek importálva.
2. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
3. Kattintson a jobb gombbal hello felső csomópont hello **hatókör** ablaktáblán, és kattintson **váltása importálja megjelenítési**.
   
    ![Válassza ki-/ bekapcsolása importálja megjelenítése](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 
4. Hello **váltása importálja megjelenítési** párbeszédpanel jelenik meg, hello hello állapotát megjelenítő importált kötet csoportok és a biztonsági másolatok. Kattintson az **OK** gombra.

Követően hello kötet csoportok és a biztonsági mentések sikeres importálásáról, használhatja a StorSimple Snapshot Manager toomanage őket, csak akkor kezelheti a kötet csoportok és a biztonsági mentések, amelyek létrehozása és konfigurálása a StorSimple Snapshot Manager. 

## <a name="refresh-connected-devices"></a>Frissítse a csatlakoztatott eszközök
A következő eljárás toosynchronize hello csatlakoztatott StorSimple eszköz StorSimple Snapshot Manager hello használata.

#### <a name="toorefresh-connected-devices"></a>toorefresh csatlakoztatott eszközök
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblán kattintson a jobb gombbal **eszközök**, és kattintson a **frissítési eszközök**. Ez szinkronizálja a hello csatlakoztatott eszközök a StorSimple Snapshot Manager így megtekintheti hello kötet csoportok és a biztonsági mentések, többek között a legutóbbi kiegészítéseket. 
   
    ![Hello StorSimple eszköz frissítése](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)

Hello **frissítési eszközök** művelet lekérdezi a csatlakoztatott eszközökről bármely új kötet csoportok és a társított biztonsági másolatok. Hello eltérően **ismételt vizsgálat kötetek** művelet hello elérhető **kötetek** csomópont, **frissítési eszközök** hello a beállításjegyzék biztonsági mentése nem állítja vissza.

## <a name="authenticate-a-device"></a>Egy eszköz hitelesítéséhez
A következő eljárás tooauthenticate a StorSimple eszköz StorSimple Snapshot Manager hello használata.

#### <a name="tooauthenticate-a-device"></a>egy eszköz tooauthenticate
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblán kattintson a **eszközök**.
3. A hello **eredmények** ablaktáblán kattintson a jobb gombbal a hello eszköz hello nevét, és kattintson a **hitelesítés**.
4. Hello **hitelesítés** párbeszédpanel jelenik meg. Írja be a hello eszköz jelszavát, és kattintson **OK**.
   
    ![Hitelesítés párbeszédpanel](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 

## <a name="view-device-details"></a>Eszköz részleteinek megtekintése
A következő eljárás tooview hello részleteit a StorSimple eszköz hello használja, és szükség esetén újraszinkronizálása hello eszköz StorSimple Snapshot Manager.

#### <a name="tooview-and-resynchronize-device-details"></a>tooview és újraszinkronizálni. eszköz részletei
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblán kattintson a **eszközök**.
3. A hello **eredmények** ablaktáblán kattintson a jobb gombbal a hello eszköz hello nevét, és kattintson a **részletek**.

4 hello **eszközadatok** párbeszédpanel jelenik meg. Ebben a mezőben látható hello nevét, modell, verzió, sorozatszámát, állapot, cél iSCSI minősített nevét (IQN), és utolsó szinkronizálás dátuma és időpontja.

* Kattintson a **újraszinkronizálásra** toosynchronize hello eszköz.
* Kattintson a **OK** vagy **Mégse** tooclose hello párbeszédpanel megnyitásához.
  
  ![Eszköz részletei](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 

## <a name="refresh-an-individual-device"></a>Egy adott eszköz frissítése
A következő eljárás tooresynchronize egy egyedi StorSimple eszközt a StorSimple Snapshot Manager hello használata.

#### <a name="toorefresh-a-device"></a>egy eszköz toorefresh
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager. 
2. A hello **hatókör** ablaktáblán kattintson a **eszközök**. 
3. A hello **eredmények** ablaktáblán kattintson a jobb gombbal a hello eszköz hello nevét, és kattintson a **frissítése eszköz**. Ez szinkronizálja a hello eszköz StorSimple Snapshot Manager.

## <a name="delete-a-device-configuration"></a>Egy eszköz konfigurálásának törlése
A következő eljárás toodelete egy egyedi StorSimple eszköz konfigurációs a StorSimple Snapshot Manager hello használata.

#### <a name="toodelete-a-device-configuration"></a>egy eszköz konfigurálásának toodelete
1. Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblán kattintson a **eszközök**. 
3. A hello **eredmények** ablaktáblán kattintson a jobb gombbal a hello eszköz hello nevét, és kattintson a **törlése**. 
4. hello a következő üzenet jelenik meg. Kattintson a **Igen** toodelete konfigurációs hello, vagy kattintson a **nem** toocancel hello törlését.
   
    ![Eszköz törlése](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Egy lejárt jelszó módosítása
Meg kell adnia egy jelszót a StorSimple eszköz StorSimple Snapshot Manager tooauthenticate. Hello Windows PowerShell felületet tooset hello eszköz használatakor konfigurálnia ezt a jelszót. Azonban hello jelszó is lejár. Ez akkor fordul elő, ha hello Azure klasszikus portál toochange hello jelszó is használhatja. Majd mivel hello eszköz StorSimple Snapshot Manager konfigurálták, mielőtt hello jelszó lejár, akkor újra eszközt kell hitelesítenie hello StorSimple Snapshot Manager.

#### <a name="toochange-hello-expired-password"></a>toochange hello lejárt a jelszava
1. A klasszikus Azure portálon hello indítsa el a hello StorSimple Manager szolgáltatást.
2. Kattintson a **eszközök** > **konfigurálása** hello eszközhöz.
3. Görgessen le a StorSimple Snapshot Manager szakasz toohello. Adjon meg egy 14 – 15 karakter hosszú jelszót. Győződjön meg arról, hogy a hello jelszó nagybetű, nagybetűk, numerikus és speciális karakterek vegyesen tartalmazza.
4. Adja meg újra a hello jelszó tooconfirm azt.
5. Kattintson a **mentése** hello lap hello alján.

#### <a name="toore-authenticate-hello-device"></a>toore-hello eszköz hitelesítéséhez
1. Indítsa el a StorSimple Snapshot Manager.
2. A hello **hatókör** ablaktáblán kattintson a **eszközök**. Konfigurált eszközök listája megjelenik hello **eredmények** ablaktáblán.
3. Válassza ki a hello eszközt, kattintson a jobb gombbal, és végül **hitelesítés**.
4. A hello **hitelesítés** ablak, írja be a hello új jelszót.
5. Válassza ki a hello eszközt, kattintson a jobb gombbal, majd válassza **frissítési eszköz**. Ez szinkronizálja a hello eszköz StorSimple Snapshot Manager.

## <a name="replace-a-failed-device"></a>Cserélje le a hibás eszköz
Ha a StorSimple eszköz sikertelen, és a készenléti (feladatátvétel) eszköz, a következő lépéseket tooconnect használata hello helyébe toohello új eszköz és a nézet hello társított biztonsági másolatok.

#### <a name="tooconnect-tooa-new-device-after-failover"></a>tooconnect tooa új eszköz feladatátvételt követően
1. Konfigurálja újra a hello iSCSI-kapcsolat toohello új eszköz. Útmutatásért lépjen túl "7. lépés: csatlakoztatási, inicializálása és egy kötet formátum" a [a helyszíni StorSimple eszköz üzembe helyezése](storsimple-8000-deployment-walkthrough-u2.md).

> [!NOTE]
> Ha hello új StorSimple eszköz hello azonos IP-címre van hello régi, előfordulhat, hogy képes tooconnect hello régi konfigurációs.


1. Állítsa le a Microsoft a StorSimple szolgáltatás hello:
   
   1. Indítsa el a Kiszolgálókezelőt.
   2. A Kiszolgálókezelő irányítópulton hello a hello **eszközök** menü **szolgáltatások**.
   3. A hello **szolgáltatások** ablakban, a select hello **Microsoft StorSimple szolgáltatás**.
   4. Hello a jobb oldali ablaktáblán, a **Microsoft StorSimple szolgáltatás**, kattintson a **hello szolgáltatás leállítása**.
2. Hello konfigurációs információkat kapcsolódó toohello régi eszköz eltávolítása:
   
   1. A Fájlkezelőben keresse meg a tooC:\ProgramData\Microsoft\StorSimple\BACatalog.
   2. Hello fájltörléssel hello BACatalog mappában.
3. Indítsa újra a Microsoft a StorSimple szolgáltatás hello:
   
   1. A Kiszolgálókezelő irányítópulton hello a hello **eszközök** menü **szolgáltatások**.
   2. A hello **szolgáltatások** ablakban, a select hello **Microsoft StorSimple szolgáltatás**.
   3. Hello a jobb oldali ablaktáblán, a **Microsoft StorSimple szolgáltatás**, kattintson a **hello szolgáltatás újraindítása**.
4. Indítsa el a StorSimple Snapshot Manager.
5. tooconfigure hello új StorSimple eszköz teljes hello lépések a 2. lépés: Csatlakozás a StorSimple eszköz [StorSimple Snapshot Manager telepítése](storsimple-snapshot-manager-deployment.md).
6. Kattintson a jobb gombbal hello legfelső szintű csomópont hello **hatókör** ablaktáblán (a StorSimple Snapshot Manager hello példában), és kattintson **váltása importálja megjelenítési**. 
7. Egy üzenet jelenik meg a hello importált kötet csoportok és a biztonsági mentések a StorSimple Snapshot Manager látható. Kattintson az **OK** gombra.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[használja a StorSimple Snapshot Manager tooadminister a StorSimple megoldásban](storsimple-snapshot-manager-admin.md).
* Ismerje meg, hogyan túl[StorSimple Snapshot Manager tooview használja, és a kötetek kezelése](storsimple-snapshot-manager-manage-volumes.md).

