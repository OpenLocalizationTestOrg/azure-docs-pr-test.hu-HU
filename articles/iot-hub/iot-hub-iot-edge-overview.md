---
title: "az Azure IoT peremhálózati aaaOverview |} Microsoft Docs"
description: "Hello architekturális alapfogalmak Azure IoT peremhálózati például átjárók, a modulok és a brókerek ismerteti."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: dobett
ms.openlocfilehash: 32debc0d4f40cfd7f2cce7cf8c76b12ec18ee2dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-edge-architectural-concepts"></a>Az Azure IoT peremhálózati architekturális fogalmak

Mielőtt bármely mintakód vizsgálja meg, vagy hozzon létre saját IoT Edge használata mező, tisztában kell lennie hello alapvető fogalmakat, amelyek IoT peremhálózati hello architektúrájának.

## <a name="iot-edge-modules"></a>Az IoT-Edge-modulok

Átjáró használjon létrehozásával és összeállításakor Azure IoT peremhálózati *IoT peremhálózati modulok*. Modulok használata *üzenetek* tooexchange adatok egymás mellett. Egy modul üzenetet kap, néhány műveletet végez, opcionálisan átalakítja az új üzenetbe, és majd közzéteszi azokat az egyéb modulok tooprocess. Egyes modulok esetleg kizárólag új üzeneteket állítanak elő, és nem dolgoznak fel beérkező üzeneteket. Modulok láncolata adatokat feldolgozó folyamat egyes átalakítás végrehajtja a hello adatokat, hogy a folyamat egy pont a modul hoz létre.

![Az átjáró moduljainak láncba rendezése az Azure IoT Edge szolgáltatással][1]

IoT peremhálózati hello a következő összetevőket tartalmazza:

* Előzetes írásbeli modulokat, amelyek a közös átjáró.
* a fejlesztői hello felületek toowrite egyéni modulok használatához.
* hello infrastruktúra szükséges toodeploy, és hajtson végre modulokat.

hello SDK absztrakciós réteget biztosít, amely lehetővé teszi toobuild átjárók toorun a különböző operációs rendszerek és platformokon.

![Az Azure IoT Edge absztrakciós rétege][2]

## <a name="messages"></a>Üzenetek

Bár végezni kapcsolatban benyújtása modulok más üzenetek tooeach egy kényelmes módszert arra tooconceptualize hogyan a átjáró működik, akkor nem tükrözik mi történik. IoT biztonsági modulok használata egy broker toocommunicate egymással. Modulok közzététele üzenetek toohello broker (például a bus, vagy a közzétételi/előfizetési üzenetkezelési mintát használ), és hagyja hello broker útvonal hello üzenet toohello modulok csatlakoztatott tooit.

A modul használja hello **Broker_Publish** toopublish üzenet toohello közvetítő működik. hello broker üzenetek tooa modul által a visszahívási függvény meghívása nyújt. Az üzenetek kulcs/érték tulajdonságokból és tartalmakból állnak, amelyek memóriablokként vannak továbbítva.

![az Azure IoT Edge Broker hello hello szerepe][3]

## <a name="message-routing-and-filtering"></a>Üzenettovábbítás és -szűrés

Két módon toodirect üzenetek toohello megfelelő IoT peremhálózati modulok:

* Hivatkozásokat tartalmaz átadhatók függvénynek adható át toohello broker, hello broker tudja hello forrás és a fogadó minden modulhoz.
* A modul végezhet üdvözlőüzenetére hello tulajdonságait.

A modul kell csak jár el egy üzenetet üdvözlőüzenetére szól, ha. Hivatkozások és a hatékony szűrési message üzenet folyamatokat létrehozni.

## <a name="next-steps"></a>Következő lépések

toosee ezekről a fogalmakról minta alkalmazott futtatja, lásd: [felfedezés Azure IoT peremhálózati architektúra][lnk-hello-world].

<!-- Images -->
[1]: media/iot-hub-iot-edge-overview/modules.png
[2]: media/iot-hub-iot-edge-overview/modules_2.png
[3]: media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md