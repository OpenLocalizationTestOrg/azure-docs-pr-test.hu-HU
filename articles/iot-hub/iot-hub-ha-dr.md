---
title: "az IoT-központ magas rendelkezésre állási és vészhelyreállítási helyreállítási aaaAzure |} Microsoft Docs"
description: "Hello Azure és az IoT-központ funkciókat, amelyek segítenek toobuild magas rendelkezésre állású Azure IoT-megoldások vész-helyreállítási képességekkel írja le."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: ae320e58-aa20-45b9-abdc-fa4faae8e6dd
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: elioda
ms.openlocfilehash: 74e1fa1682258819ffb3fd84eb79e42fc6e4120f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="iot-hub-high-availability-and-disaster-recovery"></a>Az IoT-központ magas rendelkezésre állás és katasztrófa-helyreállítás
Az Azure-szolgáltatások, mint az IoT-központ biztosítja a magas rendelkezésre ÁLLÁS létszámcsökkentések hello Azure-régiót szinten bármit tennie kellene hello megoldás által igényelt nélkül használja. hello Microsoft Azure platform is készít a vész-helyreállítási képességek vagy kereszt-régiónkénti elérhetőség megoldások szolgáltatások toohelp. Ha tooprovide globális, kereszt-régió magas rendelkezésre állás eszközöket vagy felhasználókat, tervezése és készítse elő a megoldások Azure vész-Helyreállítási funkciók tootake kihasználásához. hello cikk [Azure üzleti folytonossági műszaki útmutatót](../resiliency/resiliency-technical-guidance.md) hello beépített szolgáltatásai az Azure-ban az üzletmenet folytonosságának és vész-Helyreállítási ismerteti. Hello [vész-helyreállítási és magas rendelkezésre állás a Azure-alkalmazások] [ Disaster recovery and high availability for Azure applications] papír stratégiák architektúra-útmutatót biztosít az Azure-alkalmazások tooachieve magas rendelkezésre ÁLLÁSÚ és vész-Helyreállítási.

## <a name="azure-iot-hub-dr"></a>Az Azure IoT Hub DR
Ezenkívül toointra-régió magas rendelkezésre ÁLLÁSÚ, IoT-központ megvalósítja feladatátvételi mechanizmusok vész-helyreállítási hello felhasználói beavatkozást nem igénylő. Az IoT Hub vész-Helyreállítási önálló kezdeményezett, és a helyreállítási idő célkitűzése (RTO) rendelkezik a 2 – 26 óra és helyreállításipont-céljai (Rpo) a következő hello.

| Funkció | A HELYREÁLLÍTÁSI IDŐKORLÁT |
| --- | --- |
| A beállításjegyzék és a kommunikációs műveletek a szolgáltatás rendelkezésre állása |CName adatvesztés |
| Azonosító adataihoz az identitásjegyzékhez |0-5 perc adatvesztés |
| Az eszközről a felhőbe irányuló üzenetek |Az összes olvasatlan üzenetek elvesznek. |
| Műveletek üzenetek figyelése |Az összes olvasatlan üzenetek elvesznek. |
| Felhő-eszközre küldött üzenetek |0-5 perc adatvesztés |
| Felhő eszközre a visszajelzési üzenetsor |Az összes olvasatlan üzenetek elvesznek. |

## <a name="regional-failover-with-iot-hub"></a>Az IoT-központ regionális feladatátvétel
Az IoT-megoldások üzembe helyezési topológiájához teljes kezelés Ez a cikk hello hatókörén kívül esik. hello cikkből hello *regionális feladatátvétel* magas rendelkezésre állás és a katasztrófa utáni helyreállítás célját hello telepítési modelljét.

Egy regionális feladatátvétel modellben hello hátsó end fut megoldás elsősorban egy datacenter helyen, és egy másodlagos IoT-központ háttér, telepítve van egy másik datacenter helyen. Ha az elsődleges adatközpont hello hello IoT-központ kimaradás szenved, vagy hello eszköz toohello elsődleges adatközpont hello a hálózati kapcsolat megszakad. Eszközök hello elsődleges átjáró nem érhető el egy másodlagos végpontot használni. A kereszt-régió feladatátvételi képesség hello megoldás rendelkezésre állási túl hello egy magas rendelkezésre állása növelhető a.

Magas szinten, IoT-központ, egy regionális feladatátvétel modell tooimplement hello a következőkre lesz szüksége:

* **Egy másodlagos IoT hub és útválasztási logikai eszköz**: a szolgáltatás szüneteltetése az elsődleges régióban hello esetben eszközök kell elindítania csatlakozó tooyour másodlagos régióba. Jellegéből hello állapot-kompatibilis a legtöbb szolgáltatások, esetében gyakori, a megoldás rendszergazdák tootrigger hello közötti régió feladatátvételi folyamat. hello legjobb módja toocommunicate hello új végpont toodevices, hello folyamat irányítását megőrzésével toohave őket rendszeresen ellenőrzi a *segítõ* aktuális aktív végpont hello szolgáltatást. hello segítõ szolgáltatás lehet van replikálva, és elérhető-e tartani webalkalmazás DNS-átirányítás technikák segítségével (például az [Azure Traffic Manager][Azure Traffic Manager]).
* **Identitás beállításjegyzék replikációs** -toobe használható, hello másodlagos IoT-központ az összes eszköz identitások csatlakoztatható toohello megoldás kell tartalmaznia. hello megoldás kell biztonsági mentések georeplikált eszköz identitások tartása, és toohello másodlagos IoT-központ előtti feltöltésükhöz váltás hello aktív végpontja hello eszközökhöz. hello eszköz identitása exportálási funkció IoT hub akkor hasznos, ebben a környezetben. További információkért lásd: [IoT Hub fejlesztői útmutató - identitásjegyzékhez][IoT Hub developer guide - identity registry].
* **Az egyesítés logika** – amikor ismét elérhetővé hello elsődleges régió válik, minden hello állapotát és hello másodlagos helyen létrehozott adatok áttelepített hátsó toohello elsődleges régióban kell lennie. Ez állapotának és adatainak többnyire vonatkozik toodevice identitások és az alkalmazás metaadatainak, amely úgy kell egyesíteni hello elsődleges IoT hub és más alkalmazás-specifikus áruházak hello elsődleges régióban. toosimplify Ez a lépés idempotent műveleteket kell használnia. Az Idempotent műveletek hello mellékhatásokkal hello végleges konzisztens terjesztési események, illetve a duplikálás vagy események soron kézbesítését minimalizálása érdekében. Ezenkívül hello úgy az alkalmazáslogikát lehet tervezett tootolerate lehetséges inkonzisztenciát "némileg", dátum-állapotát. Ez a helyzet akkor következhetnek be toohello további időt hello rendszer túl "javítandó" helyreállításipont-céljai (RPO) alapján.

## <a name="next-steps"></a>Következő lépések
Hajtsa végre a további információk az Azure IoT Hub hivatkozások toolearn:

* [Ismerkedés az IoT-központok (Útmutató)][lnk-get-started]
* [Mi az Azure IoT Hub?][What is Azure IoT Hub?]

[Disaster recovery and high availability for Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Traffic Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub developer guide - identity registry]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[What is Azure IoT Hub?]: iot-hub-what-is-iot-hub.md
