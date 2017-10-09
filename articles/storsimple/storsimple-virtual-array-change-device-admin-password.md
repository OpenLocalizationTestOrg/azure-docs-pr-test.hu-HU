---
title: "aaaChange StorSimple virtuális tömb eszköz rendszergazdai jelszava |} Microsoft Docs"
description: "Ismerteti, hogyan toouse vagy hello Azure-portálon vagy a StorSimple virtuális tömb webes felhasználói felület toochange hello eszköz rendszergazdai jelszava."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 11490814-d9fd-4dc7-9c3b-55dd2c23eaf1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 531b395df7aeade0a909360797c6b0f0abd9fd1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-storsimple-virtual-array-device-administrator-password-via-storsimple-device-manager"></a>Hello StorSimple virtuális tömb eszköz rendszergazdai jelszava keresztül StorSimple Device Manager módosítása

## <a name="overview"></a>Áttekintés

Használatakor hello Windows PowerShell felületet tooaccess hello StorSimple virtuális tömb, akkor szükség tooenter egy eszköz rendszergazdai jelszava. Hello StorSimple eszköz először kiosztásakor és lépések, hello alapértelmezett jelszava *jelszó1*. Hello biztonsági adatait, hello alapértelmezett jelszava lejár, és hello első alkalommal, amikor bejelentkezik a rendszer szükséges toochange ezt a jelszót.

Az éles környezetben hello eszköz telepítését követően bármikor használhatja bármelyik hello helyi webes felhasználói felületén vagy a hello Azure portál toochange hello eszköz rendszergazdai jelszava. Ezek az eljárások leírását ebben a cikkben.

 ![Eszközök panel](./media/storsimple-virtual-array-change-device-admin-password/ova-devices-blade.png)

## <a name="use-hello-azure-portal-toochange-hello-password"></a>Hello Azure portál toochange hello jelszó használata

Hajtsa végre a következő lépéseket toochange hello eszköz rendszergazdai jelszava hello Azure-portálon keresztül hello.

#### <a name="toochange-hello-device-administrator-password-via-hello-azure-portal"></a>toochange hello eszköz rendszergazdai jelszava hello Azure-portálon keresztül

1. Hello szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és ezután belül hello **felügyeleti** kattintson **eszközök**. Ekkor megnyílik a hello **eszközök** panel, amelyen a StorSimple virtuális tömb-eszközeinek felsorolását tartalmazza.

2. A hello **eszközök** panelen kattintson duplán a jelszó megváltoztatása igénylő hello eszköz.

3. A hello **beállítások** az eszköz paneljén kattintson **biztonsági**.

4. A hello **biztonsági beállítások** panelen a következő hello:
   
   1. Görgessen lefelé toohello **eszköz rendszergazdai jelszavát** szakasz. Adjon meg egy rendszergazdai jelszót, amely a 8 too15 karaktereket tartalmaz.
   2. Hello jelszót.
   3. Kattintson a **mentése** hello panel hello tetején.

hello eszköz rendszergazdai jelszava most frissül. A módosított jelszó tooaccess hello eszköz helyileg is használhatja.

![Biztonsági beállítások panel](./media/storsimple-virtual-array-change-device-admin-password/ova-change-device-pwd.png)

## <a name="use-hello-local-web-ui-toochange-hello-password"></a>Hello helyi webes felhasználói felület toochange hello jelszó használata

Hajtsa végre a következő lépéseket toochange hello eszköz rendszergazdai jelszava hello helyi webes felhasználói felületen keresztül hello.

#### <a name="toochange-hello-device-administrator-password-via-hello-local-web-ui"></a>toochange hello eszköz rendszergazdai jelszava hello helyi webes felhasználói felületen keresztül

1. Hello helyi webes felhasználói felület, kattintson **karbantartási** > **jelszómódosítás** az eszközhöz.
   
    ![jelszó1 módosítása](./media/storsimple-virtual-array-change-device-admin-password/image40.png)
2. Adja meg a hello **jelenlegi jelszó**.
3. Adjon meg egy **új jelszó**. hello jelszó legalább 8 karakter hosszúságúnak kell lennie. 3, 4 hello következő tartalmaznia kell: nagybetűk, nagybetűk, numerikus és speciális karaktereket.
   
    Vegye figyelembe, hogy a jelszó nem lehet ugyanaz, mint a legutóbbi 24 jelszó hello hello.
4. Adja meg újból hello jelszó tooconfirm azt.
   
    ![jelszó2 módosítása](./media/storsimple-virtual-array-change-device-admin-password/image41.png)
5. Hello a hello lap alján, kattintson **alkalmaz**. Új jelszó hello most alkalmazni. Ha hello jelszó módosítása sikertelen, hiba történt a következő hello jelenik meg:
   
    ![jelszó hiba](./media/storsimple-virtual-array-change-device-admin-password/image42.png)
   
    Miután hello jelszó frissítése sikeres volt, értesítés jelenik meg. A módosított jelszó tooaccess hello eszköz helyileg majd használni.


## <a name="next-steps"></a>Következő lépések
Ismerje meg, hogyan túl[felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).

