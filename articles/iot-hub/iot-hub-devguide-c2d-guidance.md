---
title: "az IoT-központ felhő eszköz aaaAzure beállítások |} Microsoft Docs"
description: "Fejlesztői útmutató - amikor toouse közvetlen módszerek, eszköz iker kívánt tulajdonságok vagy a felhőből eszközre kommunikációhoz felhő eszközre üzenetek útmutatást."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: 1ac90923-1edf-4134-bbd4-77fee9b68d24
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: elioda
ms.openlocfilehash: bb95445054fa2711e34fc1d928c3665e0246c81c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-to-device-communications-guidance"></a>Útmutatás felhő eszközre kommunikáció
IoT Hub eszköz alkalmazások tooexpose funkció tooa háttér-alkalmazás három lehetőséget kínál:

* [Közvetlen módszerek] [ lnk-methods] hello eredményének azonnali megerősítését igénylő kommunikációhoz. Közvetlen módszereket gyakran használják az eszközök, például egy ventilátor bekapcsolása az interaktív vezérléshez.
* [Kettős a szükséges tulajdonságok] [ lnk-twins] a hosszan futó parancsokat szánt tooput hello eszköz be egy adott állapot szükséges. Állítsa például hello telemetriai adatok küldési időköze too30 perc.
* [Felhő-eszközre küldött üzenetek] [ lnk-c2d] egyirányú értesítések toohello eszköz alkalmazás.

Ez hello részletes összehasonlítása különböző felhő-és eszköz közötti kommunikációt beállításait.

|  | Közvetlen metódusok | A két a kívánt tulajdonságai | Felhő-eszközre küldött üzenetek |
| ---- | ------- | ---------- | ---- |
| Forgatókönyv | Azonnali megerősítését, például egy ventilátor bekapcsolásával igénylő parancsok. | Hosszan futó parancsokat tooput hello eszköz szánt bizonyos kívánt állapotba. Állítsa például hello telemetriai adatok küldési időköze too30 perc. | Egyirányú értesítések toohello eszközalkalmazás. |
| Adatfolyam | Kétirányú. hello eszközalkalmazás azonnal válaszolhassanak toohello metódust. hello megoldás háttérrendszerének összefüggéseikben való fogadása hello eredménye toohello kérelmet. | Egyirányú. hello eszközalkalmazás hello tulajdonság módosítását értesítést kap. | Egyirányú. hello eszközalkalmazás hello üzenetet kap.
| Tartósság | Kapcsolat nélküli eszközök nem próbál kapcsolódni. hello megoldás háttérrendszerének hello eszközökön nem kapcsolódik értesítést kap. | Tulajdonságértékek hello eszköz iker a megmaradnak. Eszköz a következő újrakapcsolódási olvasni. A tulajdonságértékek lekérhető a hello [IoT-központ lekérdezési nyelv][lnk-query]. | Üzenetek too48 órában az IoT-központ is megőrzi. |
| Cél | Egyetlen eszköz használatával **deviceId**, vagy több eszköz [feladatok][lnk-jobs]. | Egyetlen eszköz használatával **deviceId**, vagy több eszköz [feladatok][lnk-jobs]. | Egyetlen eszköz által **deviceId**. |
| Méret | Too8KB kérések és válaszok 8 KB-os. | A keresett maximális tulajdonságok mérete 8 KB-os. | Too64KB üzenetek. |
| gyakoriság | Magas. További információkért lásd: [korlátozza az IoT-központ][lnk-quotas]. | Közepes. További információkért lásd: [korlátozza az IoT-központ][lnk-quotas]. | Alacsony. További információkért lásd: [korlátozza az IoT-központ][lnk-quotas]. |
| Protokoll | Jelenleg csak akkor érhető el MQTT használatával. | Jelenleg csak akkor érhető el MQTT használatával. | Minden protokoll érhető el. Ha a HTTP-n keresztül eszközt le kell kérdeznie. |

Megtudhatja, hogyan toouse módszereket, a kívánt tulajdonságokat és a felhő-eszközre küldött üzenetek közvetlen-e meg a következő oktatóanyagok hello:

* [Közvetlen módszerekkel][lnk-methods-tutorial], közvetlen módszerek;
* [Használja a kívánt tulajdonságokkal tooconfigure eszközök][lnk-twin-properties], az eszköz iker a szükséges tulajdonságok; 
* [Felhő-eszközre küldött üzenetek küldése][lnk-c2d-tutorial], a felhő-eszközre küldött üzenetek.

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-tutorial]: iot-hub-node-node-c2d.md
