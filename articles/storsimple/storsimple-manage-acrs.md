---
title: "a StorSimple aaaManage hozzáférés-vezérlési rekordokat |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hozzáférés-vezérlés rögzíti, mely állomásokat kapcsolódhatnak tooa kötet hello StorSimple eszköz (ACRs) toodetermine."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2f1475d8-36a5-4cc4-84b9-adf8a310b60c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: a1e718c2679301b34221a233557a1eaae869a94f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-access-control-records"></a>Hello StorSimple Manager szolgáltatás toomanage access control rekordok használata
## <a name="overview"></a>Áttekintés
Hozzáférés-vezérlési rekordokat (ACRs) lehetővé teszik toospecify mely állomásokat kapcsolódhatnak tooa kötet hello StorSimple eszközön. ACRs tooa adott kötet van beállítva, és tartalmazhat hello iSCSI minősített nevét (IQN-nevekre vonatkozóan) hello gazdagépek. Ha egy állomás megpróbál tooconnect tooa kötet, hello eszköz ellenőrzi hello ACR társított hello IQN nevét, és ha egyezést, majd hello kapcsolat jön létre a köteten. hozzáférés-vezérlés hello rögzíti a hello szakasz **konfigurálása** oldal megjelenik az összes hello hozzáférés-vezérlési rekordokat hello megfelelő hello állomás IQN-nevekre vonatkozóan.

Ez az oktatóanyag azt ismerteti, a következő ACR kapcsolatos általános feladatok hello:

* Egy hozzáférés-vezérlési rekordot hozzáadása 
* Egy hozzáférés-vezérlési rekordot szerkesztése 
* Egy hozzáférés-vezérlési rekordot törlése 

> [!IMPORTANT]
> * Az ACR tooa kötet hozzárendelésekor gondoskodunk, hogy hello kötet nem egyidejűleg hozzáfér egynél több nem fürtözött gazdagép, mert ez hello kötet, megsérülhet. 
> * Ha töröl egy ACR a kötetről, ellenőrizze, hogy adott hello megfelelő gazdagép nem fér hozzá hello kötet mert hello törlését eredményezheti egy írható-olvasható megszakítása.
> 
> 

## <a name="add-an-access-control-record"></a>Egy hozzáférés-vezérlési rekordot hozzáadása
Hello StorSimple Manager szolgáltatás használ **konfigurálása** tooadd ACRs lapon. Általában egy ACR fog társítani egy kötetet.

Hajtsa végre a következő lépéseket tooadd egy ACR hello.

#### <a name="tooadd-an-access-control-record"></a>egy hozzáférés-vezérlési rekordot tooadd
1. Hello szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, majd hello **konfigurálása** fülre.
2. A táblázatos szerint felsoroló hello **hozzáférés-vezérlési rekordokat**, szállítási egy **neve** az ACR.
3. Adja meg a Windows-gazdagépen hello IQN nevét **iSCSI-kezdeményező neve**. a Windows Server-állomás IQN-Nevének tooget hello hello a következő:
   
   * Indítsa el a hello Microsoft iSCSI-kezdeményezőt a Windows-gazdagépen.
   * A hello **iSCSI-kezdeményező tulajdonságai** ablakban a hello **konfigurációs** lapra, válassza ki, másolja hello karakterlánc hello **kezdeményező neve** mező.
   * Ez a karakterlánc illessze be hello **iSCSI-kezdeményező neve** mezőjét a klasszikus Azure portálon hello hello ACRs táblán.
4. Kattintson a **mentése** toosave hello ACR, újonnan létrehozott. hello táblázatos lesz listázása kell frissített tooreflect a hozzáadást.

## <a name="edit-an-access-control-record"></a>Egy hozzáférés-vezérlési rekordot szerkesztése
Hello használata **konfigurálása** hello Azure klasszikus portál tooedit ACRs lapján. 

> [!NOTE]
> Csak a jelenleg nem használható ACRs módosíthatja. tooedit egy ACR társított olyan kötet, amely jelenleg használatban van, először érdemes hello kötet offline állapotba.
> 
> 

Hajtsa végre a következő lépéseket tooedit egy ACR hello.

#### <a name="tooedit-an-access-control-record"></a>egy hozzáférés-vezérlési rekordot tooedit
1. Hello szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, majd hello **konfigurálása** fülre.
2. Hello táblázatos felsorolása hello hozzáférés-vezérlési rekordokat, a hello ACR, hogy kívánja-e toomodify mutasson.
3. Adjon meg egy új nevet és/vagy IQN a hello ACR.
4. Kattintson a **mentése** toosave hello ACR módosítani. hello táblázatos lesz listázása kell frissített tooreflect ezt a módosítást.

## <a name="delete-an-access-control-record"></a>Egy hozzáférés-vezérlési rekordot törlése
Hello használata **konfigurálása** hello Azure klasszikus portál toodelete ACRs lapján. 

> [!NOTE]
> Csak azokat a ACRs, amelyek jelenleg nem törölhetők. toodelete egy ACR társított olyan kötet, amely jelenleg használatban van, először érdemes hello kötet offline állapotba.
> 
> 

Hajtsa végre a következő lépéseket toodelete egy hozzáférés-vezérlési rekordot hello.

#### <a name="toodelete-an-access-control-record"></a>egy hozzáférés-vezérlési rekordot toodelete
1. Hello szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, majd hello **konfigurálása** fülre.
2. A hello táblázatos felsorolása hello hozzáférés-vezérlési rekordokat (ACRs), mutasson, hogy kívánja-e toodelete ACR hello.
3. A Törlés ikonra (**x**) hello szélsőséges jobb oldali oszlopban hello kiválasztott ACR fog megjelenni. Kattintson a hello **x** ikon toodelete hello ACR.
4. Amikor felszólítja a megerősítésre, kattintson **Igen** toocontinue a hello törlését. hello táblázatos felsorolása frissített tooreflect hello törlés lesz.

## <a name="next-steps"></a>Következő lépések
* További információ [felügyelete a StorSimple-köteteket](storsimple-manage-volumes.md).
* További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

