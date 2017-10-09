---
title: "aaaDeactivate és a StorSimple eszköz törlése |} Microsoft Docs"
description: "Ismerteti, hogyan tooremove StorSimple eszközt a szolgáltatás első inaktiválása és törlését is."
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 155cda38-c5ae-45dc-b7e8-6444494afc9e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ed86bcd089aa957128e14b1709c836d938c131a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deactivate-and-delete-a-storsimple-8000-series-device-via-storsimple-manager-service"></a>Inaktiválása és a StorSimple Manager szolgáltatással a StorSimple 8000 series eszköz törlése
## <a name="overview"></a>Áttekintés
Kezdésként érdemes lehet tootake nem működik a StorSimple eszköz (például akkor, ha cseréje vagy az eszköz frissítése, vagy ha már nem használja a StorSimple). Ha ez helyzet hello, szüksége lesz toodeactivate hello eszköz törlése előtt. Hello kapcsolatot hello eszköz és a hello megfelelő StorSimple Manager szolgáltatás inaktiválása kiszolgálója. Ez az oktatóanyag azt ismerteti, hogyan tooremove először inaktiválása és majd törlésével szolgáltatásból a StorSimple eszköz. 

Ha az eszköz inaktiválása bármely hello eszközön helyileg tárolt adatok nem lesznek elérhetők. Csak a hello felhőben tárolt hello eszközhöz társított hello adatok állíthatók helyre.  

> [!WARNING]
> Az inaktiválást állandó művelet, és nem vonható vissza. Deaktivált eszköz hello StorSimple Manager szolgáltatásban nem lehet regisztrálni, kivéve, ha először a toohello alapértelmezett gyári beállításainak visszaállítása. 
> 
> hello gyári folyamat törli az eszközön helyileg tárolt hello adatok. Ezért fontos, hogy pillanatképet egy felhőalapú, az adatok egy eszköz inaktiválása előtt. Ez lehetővé teszi toorecover adatok összes hello egy későbbi időpontban.
> 
> 

Ez az oktatóanyag azt ismerteti, hogyan:

* Eszköz inaktiválása és hello adatok törlése
* Eszköz inaktiválása, így a hello adatokat

Azt is bemutatja, hogyan inaktiválása és törlési működik a StorSimple virtuális eszköz.

> [!NOTE]
> A StorSimple fizikai vagy virtuális eszköz inaktiválása előtt győződjön meg arról, hogy toostop, vagy törölje az ügyfelek és kiszolgálók azokon az eszközökön.
> 
> 

## <a name="deactivate-and-delete-data"></a>Inaktiválása és adatok törlése
Ha kíváncsi hello eszköz teljes törlése, és nem szeretné tooretain hello adatok hello eszközön, majd fejezze be a hello a következő lépéseket.

#### <a name="toodeactivate-hello-device-and-delete-hello-data"></a>toodeactivate hello eszköz és a delete hello adatok
1. Eszközök előzetes toodeactivating, törölnie kell minden hello kötet tárolók (és hello kötetek) hello eszközhöz társított. Törölheti kötettárolók csak hello társított biztonsági másolatok törlését követően.
2. Az alábbiak szerint inaktiválja hello eszköz:
   
   1. A StorSimple Manager szolgáltatás hello **eszközök** lapra, jelölje be hello eszköz, toodeactivate kívánja, hello lap hello alul kattintson **Deactivate**.
   2. Ekkor megjelenik egy megerősítő üzenet. Kattintson a **Igen** toocontinue. hello inaktiválása folyamat elindítja és eltarthat néhány percig toocomplete.
3. Az inaktiválást, után törölheti hello eszköz teljesen. Egy eszköz törlése eltávolítja a eszközök csatlakoztatott toohello szolgáltatás hello listájáról. hello szolgáltatás már nem felügyelhető hello törlése eszközt. A következő lépéseket toodelete hello eszköz hello használata:
   
   1. A StorSimple Manager szolgáltatás hello **eszközök** lapon, válassza ki, hogy kívánja-e toodelete deaktivált eszköz.
   2. Hello oldalon lent hello, kattintson a **törlése**.
   3. Megerősítést kér bekéri. Kattintson a **Igen** toocontinue.
      
      A törölt hello eszköz toobe néhány percig is eltarthat.

## <a name="deactivate-and-retain-data"></a>Inaktiválása és adatainak megőrzése
Ha érdekli a hello eszköz törlése, de tooretain hello adatokat, majd hajtsa végre a lépéseket követve hello.

#### <a name="toodeactivate-a-device-and-retain-hello-data"></a>egy eszköz toodeactivate és hello adatok megőrzése mellett
1. Hello eszköz inaktiválása. Az összes hello kötettárolók és hello pillanatképek hello eszköz maradnak.
   
   1. A StorSimple Manager szolgáltatás hello **eszközök** lapra, jelölje be hello eszköz, toodeactivate kívánja, hello lap hello alul kattintson **Deactivate**.
   2. Ekkor megjelenik egy megerősítő üzenet. Kattintson a **Igen** toocontinue. hello inaktiválása folyamat elindítja és eltarthat néhány percig toocomplete.
2. Most átveheti hello kötettárolók és a kapcsolódó hello pillanatképeket. Az eljárások, nyissa meg túl[a StorSimple eszköz feladatátvétel és a katasztrófa utáni helyreállítás](storsimple-device-failover-disaster-recovery.md).
3. Az inaktiválást és a feladatátvétel után törölheti hello eszköz teljes mértékben. Egy eszköz törlése eltávolítja a eszközök csatlakoztatott toohello szolgáltatás hello listájáról. hello szolgáltatás már nem felügyelhető hello törlése eszközt. Hajtsa végre a következő lépéseket toodelete hello eszköz hello:
   
   1. A StorSimple Manager szolgáltatás hello **eszközök** lapon, válassza ki, hogy kívánja-e toodelete deaktivált eszköz.
   2. Hello oldalon lent hello, kattintson a **törlése**.
   3. Megerősítést kér bekéri. Kattintson a **Igen** toocontinue.
      
      A törölt hello eszköz toobe néhány percig is eltarthat.

## <a name="deactivate-and-delete-a-virtual-device"></a>A virtuális eszköz inaktiváljon
A StorSimple virtuális eszköz inaktiválása felszabadítja a hello virtuális gépet. Majd törölheti hello virtuális gép és a kiépítésekor létrejött hello erőforrásokat. Hello virtuális eszközt az Inaktiválás után nem lehet visszaállítani tooits korábbi állapotába. 

A következő műveletek hello deaktiválása eredményezi:

* hello StorSimple virtuális eszközt a rendszer eltávolítja.
* hello OSDisk és adatlemezek hello StorSimple virtuális eszköz létrehozása törlődnek.
* hello üzemeltetett szolgáltatás és a kiépítés során létrehozott virtuális hálózat megmarad. Ha nem használ ezeket az entitásokat, akkor manuálisan kell törölnie őket.
* Hello StorSimple virtuális eszköz által létrehozott felhőalapú pillanatfelvételek megmaradnak.

## <a name="next-steps"></a>Következő lépések
* toorestore hello deaktivált eszköz toofactory alapértelmezett beállításokat, nyissa meg túl[hello eszköz toofactory alapértelmezett beállításainak visszaállítása](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* Technikai támogatásért [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md).
* További részletek toolearn, hogyan lépjen toouse hello StorSimple Manager szolgáltatás, túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md). 

