---
title: "aaaDeactivate, és törölje a Microsoft Azure StorSimple virtuális tömb |} Microsoft Docs"
description: "Ismerteti, hogyan tooremove StorSimple eszközt a szolgáltatás első inaktiválása és törlését is."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: a929f5bc-03e2-4b01-b925-973db236f19f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: b1f3ddb5822d19965739777e238af19b507df984
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a>A StorSimple virtuális tömb inaktiváljon

## <a name="overview"></a>Áttekintés

Ha egy StorSimple virtuális tömb inaktiválja, megszakítja hello kapcsolat hello eszköz és hello megfelelő StorSimple Device Manager szolgáltatás között. Ez az oktatóanyag azt ismerteti, hogyan:

* Eszköz inaktiválása 
* Deaktivált eszköz törlése

a cikkben szereplő információkat hello virtuális tömbök tooStorSimple vonatkozik. 8000 Series információkért nyissa meg toohow túl[inaktiválja vagy törölje az eszköz](storsimple-deactivate-and-delete-device.md).

## <a name="when-toodeactivate"></a>Amikor toodeactivate?

Az inaktiválást állandó művelet, és nem vonható vissza. Deaktivált eszköz újra hello StorSimple Device Manager szolgáltatás nem regisztrált. Előfordulhat, hogy toodeactivate kell, és törölje a következő forgatókönyvek hello egy StorSimple virtuális tömb:

* **A tervezett feladatátvételt** : az eszköz online állapotban, és azt tervezi, toofail keresztül az eszközt. Ha azt tervezi, tooupgrade tooa nagyobb eszközre, szükség lehet toofail keresztül az eszközt. Miután hello adatok tulajdonjoga kerül, és hello feladatátvétel befejeződött, a rendszer automatikusan törli a hello forráseszközt.
* **Nem tervezett feladatátvétel** : az eszköz offline állapotban, és szüksége toofail hello eszközön keresztül. Ez a forgatókönyv során egy olyan vészhelyzet esetén fordulhat, ha nincs kimaradás hello adatközpontban, és az elsődleges eszköz nem működik. Toofail hello eszköz tooa másodlagos eszköz keresztül tervezi. Miután hello adatok tulajdonjoga kerül, és hello feladatátvétel befejeződött, a rendszer automatikusan törli a hello forráseszközt.
* **Szerelje le** : kívánt toodecommission hello eszköz. Ehhez toofirst hello eszköz inaktiválása, és törölje azt. Ha egy eszköz inaktiválja, már nem érheti el helyben tárolt adatokat. Egyetlen hozzáférés és a helyreállítás hello felhőben tárolt adatok hello is. Ha tookeep hello eszközadatok inaktiválás után, majd kell venni az adatok egy felhő-pillanatfelvételt eszköz inaktiválása előtt. A felhő-pillanatfelvételt toorecover lehetővé teszi az összes adat hello egy későbbi időpontban.

## <a name="deactivate-a-device"></a>Eszköz inaktiválása

toodeactivate eszközét, hajtsa végre az alábbi lépésekkel hello.

#### <a name="toodeactivate-hello-device"></a>toodeactivate hello eszköz

1. A szolgáltatást, lépjen túl**felügyeleti > eszközök**. A hello **eszközök** panelen, kattintson és, hogy kívánja-e toodeactivate válassza hello eszköz.
   
    ![Válassza ki az eszköz toodeactivate](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete7.png)
2. Az a **eszköz irányítópult** paneljén kattintson **... További** hello listájából válassza ki és **Deactivate**.
   
    ![Kattintson az Inaktiválás gombra](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete8.png)
3. A hello **Deactivate** paneljén, típusnév hello eszköz, és kattintson **Deactivate**. 
   
    ![Erősítse meg inaktiválása](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete1.png)
   
    hello inaktiválása folyamat elindul, és néhány perc toocomplete vesz igénybe.
   
    ![A folyamatban lévő inaktiválása](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete2.png)
4. Az inaktiválást, után hello listán szereplő eszközök frissíti.
   
    ![Teljes inaktiválása](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete3.png)
   
    Most törölheti ezt az eszközt.

## <a name="delete-hello-device"></a>Hello eszköz törlése

Egy eszköz rendelkezik toobe első deaktivált toodelete azt. Egy eszköz törlése eltávolítja a eszközök csatlakoztatott toohello szolgáltatás hello listájáról. hello szolgáltatás már nem felügyelhető hello törlése eszközt. hello adatok hello eszközhöz társított azonban hello felhőben marad. Ezek az adatok a díjak majd fizetni.

toodelete hello eszköz, hajtsa végre az alábbi lépésekkel hello.

#### <a name="toodelete-hello-device"></a>toodelete hello eszköz

1. Túl nyissa meg a StorSimple Device Manager**felügyeleti > eszközök**. A hello **eszközök** panelen jelölje ki az, hogy kívánja-e toodelete deaktivált eszközt.
2. A hello **eszköz irányítópult** paneljén kattintson **... További** majd **törlése**.
   
   ![Válassza ki az eszköz toodelete](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete4.png)
3. A hello **törlése** paneljén, típusnév hello az eszköz tooconfirm hello törlésre, és kattintson a **törlése**. Hello-eszköz törlése nem törli hello eszközhöz társított hello felhőbeli adatát. 
   
   ![Törlés megerősítése](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete5.png) 
4. hello törlése elindul, és néhány perc toocomplete vesz igénybe.
   
   ![Törlése folyamatban](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete6.png)
   
    Hello eszköz törlését követően megtekintheti az eszközök listájának frissítése hello.

## <a name="next-steps"></a>Következő lépések

* Információ toofail keresztül, nyissa meg túl[feladatátvétel és a katasztrófa utáni helyreállítás a StorSimple virtuális tömb](storsimple-virtual-array-failover-dr.md).

* További részletek toolearn, hogyan toouse hello StorSimple Device Manager szolgáltatást, nyissa meg túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple virtuális tömb](storsimple-virtual-array-manager-service-administration.md). 

