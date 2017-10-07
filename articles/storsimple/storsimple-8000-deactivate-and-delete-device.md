---
title: "aaaDeactivate és a StorSimple 8000 series eszköz törlése |} Microsoft Docs"
description: "Ismerteti, hogyan tooremove StorSimple eszközt a szolgáltatás első inaktiválása és törlését is."
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
ms.date: 05/23/2017
ms.author: alkohli
ms.openlocfilehash: 841ecd7f0fb5e425bf23e1fe0044faeab2af4b53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deactivate-and-delete-a-storsimple-device"></a>A StorSimple eszköz inaktiváljon

## <a name="overview"></a>Áttekintés

Ez a cikk ismerteti, hogyan toodeactivate, és törölje a StorSimple eszköz, amely csatlakoztatott tooa StorSimple Device Manager szolgáltatást. Ez a cikk útmutatást hello csak tooStorSimple 8000 sorozat eszközeire hello StorSimple felhő készülékek beleértve vonatkozik. Ha egy StorSimple virtuális tömb használ, majd nyissa meg túl[inaktiválását vagy törlését a StorSimple virtuális tömb](storsimple-virtual-array-deactivate-and-delete-device.md).

Az inaktiválást kiszolgálója hello kapcsolat hello eszköz és hello megfelelő StorSimple Device Manager szolgáltatás között. Kezdésként érdemes lehet tootake nem működik a StorSimple eszköz (például akkor, ha cseréje vagy az eszköz frissítése, vagy ha már nem használja a StorSimple). Ha igen, van szüksége toodeactivate hello eszköz törlése előtt.

Ha az eszköz inaktiválása hello eszközön helyileg tárolt adatokat már nem érhető el. Csak a hello felhőben tárolt hello eszközhöz társított hello adatok állíthatók helyre.

> [!WARNING]
> Az inaktiválást állandó művelet, és nem vonható vissza. Deaktivált eszköz nem regisztrálható a hello StorSimple Device Manager szolgáltatás, kivéve, ha toofactory Alapértelmezések visszaállítása.
>
> hello gyári folyamat törli az eszközön helyileg tárolt hello adatok. Ezért kell pillanatképet egy felhőalapú, az adatok eszköz inaktiválása előtt. A felhő-pillanatfelvételt toorecover lehetővé teszi az összes adat hello egy későbbi időpontban.

Ez az oktatóanyag elolvasása, után fogja tudni:

* Eszköz inaktiválása és hello adatok törlése.
* Eszköz inaktiválása és hello adatok megőrzése mellett.

> [!NOTE]
> A StorSimple fizikai eszköz vagy a felhő készülék inaktiválása előtt állítsa le, vagy törli az ügyfelek és kiszolgálók azokon az eszközökön.


## <a name="deactivate-and-delete-data"></a>Inaktiválása és adatok törlése

Ha kíváncsi hello eszköz teljes törlése, és nem szeretné tooretain hello adatok hello eszközön, majd fejezze be a hello a következő lépéseket.

#### <a name="toodeactivate-hello-device-and-delete-hello-data"></a>toodeactivate hello eszköz és a delete hello adatok

1. Eszköz inaktiválása előtt törölnie kell minden hello kötet tárolók (és hello kötetek) hello eszközhöz társított. Törölheti kötettárolók csak hello társított biztonsági másolatok törlését követően.

    > [!NOTE]
    > A StorSimple fizikai eszköz vagy a felhő készülék inaktiválásához előtt győződjön meg arról, hogy törölt hello kötettároló hello adatait ténylegesen töröltek, amelyek hello eszköz. Hello felhőben felhasználási diagramok egyaránt képes figyelni, és amikor hello felhőalapú használata miatt a törölt hello biztonsági mentések dobja el, majd folytassa toodeactivate hello eszköz. Ha előtt hello eszköz inaktiválása az vetett következik be, hello adatok hello tárfiókban lévő stranded van, és díjak fizetni.

2. Az alábbiak szerint inaktiválja hello eszköz:
   
   1. Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **eszközök**. A hello **eszközök** panelen, jelölje be hello eszközt, hogy kívánja-e toodeactivate, kattintson a jobb gombbal, és kattintson **Deactivate**.

        ![A StorSimple eszköz inaktiválása](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. A hello **Deactivate** panelen írja be a hello eszköz neve tooconfirm, és kattintson a **Deactivate**. hello inaktiválása folyamat elindul, és néhány perc toocomplete vesz igénybe.

        ![A StorSimple eszköz inaktiválása](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)

3. Az inaktiválást, után törölheti hello eszköz teljesen. Egy eszköz törlése eltávolítja a eszközök csatlakoztatott toohello szolgáltatás hello listájáról. hello szolgáltatás már nem felügyelhető hello törlése eszközt. A következő lépéseket toodelete hello eszköz hello használata:
   
   1. Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **eszközök**. A hello **eszközök** panelen, toodelete, kattintson a jobb gombbal, és végül kívánja inaktiválása válassza hello eszköz **törlése**.

        ![A StorSimple eszköz inaktiválása](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. A hello **törlése** panelen írja be a hello eszköz neve tooconfirm, és kattintson a **törlése**. hello törlés néhány perc toocomplete vesz igénybe.

        ![A StorSimple eszköz inaktiválása](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Miután hello törlése sikeresen befejeződött, értesítés jelenik meg. hello eszközlista tooreflect hello törlését is frissíti.

## <a name="deactivate-and-retain-data"></a>Inaktiválása és adatainak megőrzése

Ha érdekli a hello eszköz törlése, de tooretain hello adatokat, majd hajtsa végre a lépéseket követve hello:

#### <a name="toodeactivate-a-device-and-retain-hello-data"></a>egy eszköz toodeactivate és hello adatok megőrzése mellett
1. Hello eszköz inaktiválása. Az összes hello kötettárolók és hello pillanatképek hello eszköz maradnak.
   
   1. Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **eszközök**. A hello **eszközök** panelen, jelölje be hello eszközt, hogy kívánja-e toodeactivate, kattintson a jobb gombbal, és kattintson **Deactivate**.

         ![A StorSimple eszköz inaktiválása](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. A hello **Deactivate** panelen írja be a hello eszköz neve tooconfirm, és kattintson a **Deactivate**. hello inaktiválása folyamat elindul, és néhány perc toocomplete vesz igénybe.

         ![A StorSimple eszköz inaktiválása](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)
2. Most átveheti hello kötettárolók és a kapcsolódó hello pillanatképeket. Az eljárások, nyissa meg túl[a StorSimple eszköz feladatátvétel és a katasztrófa utáni helyreállítás](storsimple-8000-device-failover-disaster-recovery.md).
3. Az inaktiválást és a feladatátvétel után törölheti hello eszköz teljes mértékben. Egy eszköz törlése eltávolítja a eszközök csatlakoztatott toohello szolgáltatás hello listájáról. hello szolgáltatás már nem felügyelhető hello törlése eszközt. toodelete hello eszköz, a teljes hello a következő lépéseket:
   
   1. Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **eszközök**. A hello **eszközök** panelen, toodelete, kattintson a jobb gombbal, és végül kívánja inaktiválása válassza hello eszköz **törlése**.

       ![A StorSimple eszköz inaktiválása](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. A hello **törlése** panelen írja be a hello eszköz neve tooconfirm, és kattintson a **törlése**. hello törlés néhány perc toocomplete vesz igénybe.

       ![A StorSimple eszköz inaktiválása](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Miután hello törlése sikeresen befejeződött, értesítés jelenik meg. hello eszközlista tooreflect hello törlését is frissíti.

     
## <a name="deactivate-and-delete-a-cloud-appliance"></a>A felhő készülék inaktiváljon

A StorSimple felhő alapplatformjaként hello portálról az inaktiválást felszabadítja a, és törli hello virtuális gépet, és a kiépítésekor létrejött hello erőforrásokat. Hello felhő készüléknek az Inaktiválás után nem lehet visszaállítani tooits korábbi állapotába.

![StorSimple felhő készülék inaktiválása](./media/storsimple-8000-deactivate-and-delete-device/deactivate7.png)

A következő műveletek hello deaktiválása eredményezi:

* StorSimple felhő készülék hello hello szolgáltatás törlődik.
* StorSimple felhő készülék hello hello virtuális gép törlődik.
* hello operációsrendszer-lemez és adatlemezek hello StorSimple felhő készülék készült törlődnek.
* hello üzemeltetett szolgáltatás és a kiépítés során létrehozott virtuális hálózati jelennek meg. Ha nem használ ezeket az entitásokat, akkor manuálisan kell törölnie őket.
* StorSimple felhő készülék hello által létrehozott felhőalapú pillanatfelvételek megmaradnak.

Hello felhő készüléknek az Inaktiválás után törölheti hello eszközök listája. Jelölje be hello inaktiválása eszközt, kattintson a jobb gombbal, és kattintson **törlése**. StorSimple értesítést küld, miután hello eszköz törlődik, és hello eszközök frissítések listája.

## <a name="next-steps"></a>Következő lépések

* toorestore hello deaktivált eszköz toofactory alapértelmezett beállításokat, nyissa meg túl[hello eszköz toofactory alapértelmezett beállításainak visszaállítása](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* Technikai támogatásért [forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md).
* További részletek toolearn, hogyan toouse hello StorSimple Device Manager szolgáltatást, nyissa meg túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

