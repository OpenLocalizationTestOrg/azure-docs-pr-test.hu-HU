---
title: "aaaManage hozzáférés-vezérlési rekordokat a StorSimple virtuális tömb |} Microsoft Docs"
description: "Ismerteti, hogyan toomanage hozzáférés-vezérlés rögzíti, mely állomásokat csatlakozni tud a StorSimple virtuális tömb hello tooa köteten (ACRs) toodetermine."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: e154bb4f-faab-4d92-a593-900c3ddc9595
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 608fdf72413761ce3c9c4bf297a748489c415685
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-toomanage-access-control-records-for-storsimple-virtual-array"></a>StorSimple az Eszközkezelő toomanage hozzáférés-vezérlési rekordokat a StorSimple virtuális tömb

## <a name="overview"></a>Áttekintés

Hozzáférés-vezérlési rekordokat (ACRs) lehetővé teszik toospecify mely állomásokat kapcsolódhatnak tooa köteten hello StorSimple virtuális tömb (más néven hello StorSimple a helyszíni virtuális eszközt használ). ACRs tooa adott kötet van beállítva, és tartalmazhat hello iSCSI minősített nevét (IQN-nevekre vonatkozóan) hello gazdagépek. Ha egy állomás megpróbál tooconnect tooa kötet, hello eszköz ellenőrzi, hogy a köteten hello IQN nevének társított ACR hello, és ha van egyezés, majd hello kapcsolat létrejön. Hello **hozzáférés-vezérlési rekordokat** belül hello panel **konfigurációs** szakasz az Eszközkezelő szolgáltatás az összes hello hozzáférés-vezérlési rekordokat rendelkező hello megfelelő hello állomás IQN-nevekre vonatkozóan jeleníti meg.

![Hozzáférés-vezérlési rekordokat kezelése](./media/storsimple-virtual-array-manage-acrs/ova-manage-acrs.png)

Ez az oktatóanyag azt ismerteti, a következő ACR kapcsolatos általános feladatok hello:

* Hello IQN-Nevének lekérése
* Egy hozzáférés-vezérlési rekordot hozzáadása
* Egy hozzáférés-vezérlési rekordot szerkesztése
* Egy hozzáférés-vezérlési rekordot törlése

> [!IMPORTANT]
> 
> * Az ACR tooa kötet hozzárendelésekor gondoskodunk, hogy hello kötet nem egyidejűleg hozzáfér egynél több nem fürtözött gazdagép, mert ez hello kötet, megsérülhet.
> * Ha töröl egy ACR a kötetről, ellenőrizze, hogy adott hello megfelelő gazdagép nem fér hozzá hello kötet mert hello törlését eredményezheti egy írható-olvasható megszakítása.


## <a name="get-hello-iqn"></a>Hello IQN-Nevének lekérése

Hajtsa végre a következő lépéseket tooget hello egy Windows Server 2012 rendszert futtató Windows-állomás IQN-Nevének hello.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Egy ACR hozzáadása

Használhat **hozzáférés-vezérlési rekordokat** belül hello panel **konfigurációs** a StorSimple Device Manager szolgáltatás tooadd ACRs szakasza. Általában egy ACR hozzárendel egy kötetet.

Egy mennyiségi egy ACR társításával kapcsolatos információkért nyissa meg túl[kötet hozzáadása](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).

> [!IMPORTANT]
> Az ACR tooa kötet hozzárendelésekor gondoskodunk, hogy hello kötet nem egyidejűleg hozzáfér egynél több nem fürtözött gazdagép, mert ez hello kötet, megsérülhet.


Hajtsa végre a következő lépéseket tooadd egy ACR hello.

#### <a name="tooadd-an-acr"></a>egy ACR tooadd

1. Hello szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és ezután belül hello **konfigurációs** kattintson **hozzáférés-vezérlési rekordokat**.
2. A hello **hozzáférés-vezérlési rekordokat** panelen kattintson a **Hozzáadás**.
3. A hello **hozzáadása ACR** panelen a következő hello:
   
    1. Adja meg az ACR **nevét**.
    
    2. A **iSCSI-kezdeményező neve**, adja meg a Windows-állomás IQN hello nevét. a Windows Server-állomás IQN-Nevének tooget hello hello a következő:
   
    3. Indítsa el a hello Microsoft iSCSI-kezdeményezőt a Windows-gazdagépen. Hello iSCSI kezdeményező tulajdonságai ablakban a hello **konfigurációs** lapra, válassza ki, másolja hello karakterlánc hello **kezdeményező neve** mező.
    Illessze be ezt a karakterláncot a hello **IQN** hello mezőjét **hozzáadása ACR** panelen.
   
    6. Kattintson a **Hozzáadás** tooadd hello ACR.  
   
        ![Hozzáférés-vezérlési rekordokat hozzáadása](./media/storsimple-virtual-array-manage-acrs/ova-add-acrs.png)
4. táblázatos felsorolása hello van frissített tooreflect a hozzáadást.

## <a name="edit-an-acr"></a>Egy ACR szerkesztése

Hello használata **hozzáférés-vezérlési rekordokat** belül hello panel **konfigurációs** az Eszközkezelő szolgáltatás az Azure portál tooedit ACRs hello szakasza.

> [!NOTE]
> Ne módosítsa az ACR, amely jelenleg használatban van. tooedit egy ACR társított olyan kötet, amely jelenleg használatban van, először hajtson hello kötet offline állapotba.


Hajtsa végre a következő lépéseket tooedit egy ACR hello.

#### <a name="tooedit-an-acr"></a>egy ACR tooedit

1. Hello szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és ezután belül hello **konfigurációs** szakaszban **hozzáférés-vezérlési rekordokat**.
2. A hello **hozzáférés-vezérlési rekordokat** paneljén hello táblázatos felsorolása hello hozzáférés-vezérlési rekordokat, kattintson duplán a hello ACR, hogy kívánja-e toomodify.
3. A hello **Szerkesztés hozzáférés-vezérlési rekordokat** panelen a következő hello:
   
    1. Adja meg az hello ACR IQN hello.
   
    2. Kattintson a **mentése** hello panel toosave hello tetején hello ACR módosítani. A következő megerősítő üzenetet hello jelenik meg:
   
        ![Hozzáférés-vezérlési rekordokat szerkesztése](./media/storsimple-virtual-array-manage-acrs/ova-edit-acrs.png)

## <a name="delete-an-access-control-record"></a>Egy hozzáférés-vezérlési rekordot törlése

Hello használata **konfigurációs** hello Azure portál toodelete ACRs lapján.

> [!NOTE]
> 
> * Ne törölje az ACR, amely jelenleg használatban van. toodelete egy ACR társított olyan kötet, amely jelenleg használatban van, először hajtson hello kötet offline állapotba.
> * Ha töröl egy ACR a kötetről, ellenőrizze, hogy adott hello megfelelő gazdagép nem fér hozzá hello kötet mert hello törlését eredményezheti egy írható-olvasható megszakítása.


Hajtsa végre a következő lépéseket toodelete egy hozzáférés-vezérlési rekordot hello.

#### <a name="toodelete-an-access-control-record"></a>egy hozzáférés-vezérlési rekordot toodelete

1. Hello szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és ezután belül hello **konfigurációs** szakaszban **hozzáférés-vezérlési rekordokat**.

2. A hello **hozzáférés-vezérlési rekordokat** paneljén hello táblázatos felsorolása hello hozzáférés-vezérlési rekordokat, kattintson duplán a hello ACR, hogy kívánja-e toodelete.

3. Hello Szerkesztés hozzáférést vezérlő rekordok paneljén kattintson **törlése**.
   
    ![ACRS törlése](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs.png)

4. Amikor felszólítja a megerősítésre, kattintson **törlése** toocontinue a hello törlését. hello táblázatos felsorolása frissített tooreflect hello törlésre.
   
   ![Figyelmeztető üzenet](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs-warning.png)

## <a name="next-steps"></a>Következő lépések

* További információ [kötetek hozzáadásával és konfigurálásával ACRs](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).

