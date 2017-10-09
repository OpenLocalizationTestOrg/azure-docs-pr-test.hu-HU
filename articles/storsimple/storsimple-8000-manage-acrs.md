---
title: "a StorSimple aaaManage hozzáférés-vezérlési rekordokat |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hozzáférés-vezérlés rögzíti, mely állomásokat kapcsolódhatnak tooa kötet hello StorSimple eszköz (ACRs) toodetermine."
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
ms.date: 05/31/2017
ms.author: alkohli
ms.openlocfilehash: cf532206e2c0bc49da853663ba34ae993ec2981d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-access-control-records"></a>Hello StorSimple Manager szolgáltatás toomanage access control rekordok használata

## <a name="overview"></a>Áttekintés
Hozzáférés-vezérlési rekordokat (ACRs) lehetővé teszik toospecify mely állomásokat kapcsolódhatnak tooa kötet hello StorSimple eszközön. ACRs tooa adott kötet van beállítva, és tartalmazhat hello iSCSI minősített nevét (IQN-nevekre vonatkozóan) hello gazdagépek. Ha egy állomás megpróbál tooconnect tooa kötet, hello eszköz ellenőrzi hello ACR társított hello IQN nevét, és ha egyezést, majd hello kapcsolat jön létre a köteten. hozzáférés-vezérlési rekordokat a hello hello **konfigurációs** a StorSimple Device Manager szolgáltatás panelre szakasza az összes hello hozzáférés-vezérlési rekordokat hello állomás IQN-nevekre vonatkozóan megfelelő hello együtt jelennek meg.

Ez az oktatóanyag azt ismerteti, a következő ACR kapcsolatos általános feladatok hello:

* Egy hozzáférés-vezérlési rekordot hozzáadása
* Egy hozzáférés-vezérlési rekordot szerkesztése
* Egy hozzáférés-vezérlési rekordot törlése

> [!IMPORTANT]
> * Az ACR tooa kötet hozzárendelésekor gondoskodunk, hogy hello kötet nem egyidejűleg hozzáfér egynél több nem fürtözött gazdagép, mert ez hello kötet, megsérülhet.
> * Ha töröl egy ACR a kötetről, ellenőrizze, hogy adott hello megfelelő gazdagép nem fér hozzá hello kötet mert hello törlését eredményezheti egy írható-olvasható megszakítása.

## <a name="get-hello-iqn"></a>Hello IQN-Nevének lekérése

Hajtsa végre a következő lépéseket tooget hello egy Windows Server 2012 rendszert futtató Windows-állomás IQN-Nevének hello.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]


## <a name="add-an-access-control-record"></a>Egy hozzáférés-vezérlési rekordot hozzáadása
Hello használata **konfigurációs** hello StorSimple Device Manager szolgáltatás panel tooadd ACRs szakaszában. Általában egy ACR fog társítani egy kötetet.

Hajtsa végre a következő lépéseket tooadd egy ACR hello.

#### <a name="tooadd-an-acr"></a>egy ACR tooadd

1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és ezután belül hello **konfigurációs** kattintson **hozzáférés-vezérlési rekordokat**.
2. A hello **hozzáférés-vezérlési rekordokat** panelen kattintson a **+ Hozzáadás ACR**.

    ![Kattintson a Hozzáadás ACR](./media/storsimple-8000-manage-acrs/createacr1.png)

3. A hello **hozzáadása ACR** panelen hello a következő lépéseket:

    1. Adja meg az ACR nevét.
    
    2. Adja meg a Windows Server-állomáson alatt hello IQN nevét **iSCSI kezdeményező nevét (IQN)**.

    3. Kattintson a **Hozzáadás** toocreate hello ACR.

        ![Kattintson a Hozzáadás ACR](./media/storsimple-8000-manage-acrs/createacr2.png)

4.  hello újonnan hozzáadott ACR hello ACRs táblázatos listáját jeleníti meg.

    ![Kattintson a Hozzáadás ACR](./media/storsimple-8000-manage-acrs/createacr5.png)


## <a name="edit-an-access-control-record"></a>Egy hozzáférés-vezérlési rekordot szerkesztése
Hello használata **konfigurációs** hello StorSimple Device Manager szolgáltatás panel tooedit ACRs szakaszában.

> [!NOTE]
> Javasoljuk, hogy csak a jelenleg nem használható ACRs módosítása. tooedit egy ACR társított olyan kötet, amely jelenleg használatban van, először érdemes hello kötet offline állapotba.

Hajtsa végre a következő lépéseket tooedit egy ACR hello.

#### <a name="tooedit-an-access-control-record"></a>egy hozzáférés-vezérlési rekordot tooedit
1.  Nyissa meg tooyour StorSimple Device Manager szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és ezután belül hello **konfigurációs** kattintson **hozzáférés-vezérlési rekordokat**.

    ![Nyissa meg tooaccess vezérlésére rekordok](./media/storsimple-8000-manage-acrs/createacr1.png)

2. Kattintson a hello táblázatos felsorolása hello hozzáférés-vezérlési rekordokat, és válassza ki, hogy kívánja-e toomodify ACR hello.

    ![Hozzáférés-vezérlési rekordokat szerkesztése](./media/storsimple-8000-manage-acrs/editacr1.png)

3. A hello **Szerkesztés hozzáférés-vezérlési rekordot** panelen adjon meg egy másik IQN-Nevének megfelelő tooanother állomás.

    ![Hozzáférés-vezérlési rekordokat szerkesztése](./media/storsimple-8000-manage-acrs/editacr2.png)

4. Kattintson a **Save** (Mentés) gombra. Ha a rendszer megerősítést kér, kattintson az **Igen** gombra. 

    ![Hozzáférés-vezérlési rekordokat szerkesztése](./media/storsimple-8000-manage-acrs/editacr3.png)

5. Értesítést kap hello ACR frissül. hello táblázatos felsorolása tooreflect hello változás is frissíti.

   
## <a name="delete-an-access-control-record"></a>Egy hozzáférés-vezérlési rekordot törlése
Hello használata **konfigurációs** hello StorSimple Device Manager szolgáltatás panel toodelete ACRs szakaszában.

> [!NOTE]
> Csak azokat a ACRs, amelyek jelenleg nem törölhetők. toodelete egy ACR társított olyan kötet, amely jelenleg használatban van, először érdemes hello kötet offline állapotba.

Hajtsa végre a következő lépéseket toodelete egy hozzáférés-vezérlési rekordot hello.

#### <a name="toodelete-an-access-control-record"></a>egy hozzáférés-vezérlési rekordot toodelete
1.  Nyissa meg tooyour StorSimple Device Manager szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és ezután belül hello **konfigurációs** kattintson **hozzáférés-vezérlési rekordokat**.

    ![Nyissa meg tooaccess vezérlésére rekordok](./media/storsimple-8000-manage-acrs/createacr1.png)

2. Kattintson a hello táblázatos felsorolása hello hozzáférés-vezérlési rekordokat, és válassza ki, hogy kívánja-e toodelete ACR hello.

    ![Nyissa meg tooaccess vezérlésére rekordok](./media/storsimple-8000-manage-acrs/deleteacr1.png)

3. Kattintson a jobb gombbal a tooinvoke hello helyi menüt, és válassza ki **törlése**.

    ![Nyissa meg tooaccess vezérlésére rekordok](./media/storsimple-8000-manage-acrs/deleteacr2.png)

4. Megerősítést kér, amikor hello tekintse át, és kattintson a **törlése**.

    ![Nyissa meg tooaccess vezérlésére rekordok](./media/storsimple-8000-manage-acrs/deleteacr3.png)

5. Hello törlés befejezéséről értesítést kap. hello táblázatos felsorolása frissített tooreflect hello törlésre.

    ![Nyissa meg tooaccess vezérlésére rekordok](./media/storsimple-8000-manage-acrs/deleteacr5.png)

## <a name="next-steps"></a>Következő lépések
* További információ [felügyelete a StorSimple-köteteket](storsimple-8000-manage-volumes-u2.md).
* További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

