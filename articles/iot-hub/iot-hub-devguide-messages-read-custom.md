---
title: "aaaUnderstand Azure IoT Hub egyéni végpontokat |} Microsoft Docs"
description: "Fejlesztői útmutató - szabályok alkalmazásával a útválasztási tooroute eszköz a felhőbe küldött üzeneteket toocustom végpontok."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: daa9cfb35d0853e316bbf469b034d4dadbd4e85d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>Üzenet útvonalak és egyéni végpontokat használja az eszköz a felhőbe küldött üzeneteket

Az IoT-központ lehetővé teszi a tooroute [eszköz a felhőbe küldött üzeneteket] [ lnk-device-to-cloud] tooIoT központ szolgáltatás felé néző végpontok üzenet tulajdonságai alapján. Üzenetek, hová kell toogo hello nélkül további szolgáltatások tooprocess üzenetek vagy toowrite kód útválasztási szabályok adja meg azt a rugalmasságot toosend hello szüksége. Egyes útválasztási szabályokat konfigurál a következő tulajdonságai hello rendelkezik:

| Tulajdonság      | Leírás |
| ------------- | ----------- |
| **Name (Név)**      | hello egyedi nevet, amely azonosítja a hello szabály. |
| **Forrás**    | hello adatok származásának hello reakciót toobe adatfolyam. Például telemetriát. |
| **Az állapot** | hello lekérdezési kifejezésben hello útválasztási szabály hello üzenet fejlécek és body futtatni, és a használt toodetermine, hogy hello végpont egyezés-e. Útvonal feltétel létrehozásával kapcsolatos további információkért lásd: hello [referencia - lekérdezési nyelv eszköz twins és feladatok][lnk-devguide-query-language]. |
| **Végpont**  | ahol az IoT-központ küldi hello feltételének üzenetek hello végpont hello nevét. Végpontok kell hello hello IoT hub és ugyanabban a régióban, ellenkező esetben akkor számítható a kereszt-régió írja. |

Egyetlen üzenet előfordulhat, hogy megfelelő hello feltétel több útválasztási szabályok eset az IoT-központ szolgáltatás nyújt hello üzenet toohello végponthoz társított minden egyező szabályt. Az IoT-központ is automatikusan deduplicates üzenetkézbesítést, így ha egy állapotüzenet megfelel a több szabályok hello rendelkező ugyanarra a célgépre, csak írás toothat cél egyszer.

Az IoT-központ rendelkezik egy alapértelmezett [beépített végpont][lnk-built-in]. Egyéni végpontokat tooroute üzenetek tooby létrehozhatja, ha az előfizetés toohello központban más szolgáltatások is létrehozhat. Az IoT-központ jelenleg Eseményközpontokhoz, a Service Bus-üzenetsorok és a Service Bus-üzenettémakörök egyéni végpontként.

> [!WARNING]
> Service Bus-üzenetsorok és témakörök a **munkamenetek** vagy **ismétlődő észlelési** engedélyezve van az egyéni végpontok nem támogatottak.

Egyéni végpontokat az IoT hubon létrehozásával kapcsolatos további információkért lásd: [IoT-központok végpontjai][lnk-devguide-endpoints].

A egyéni végpontok olvasását kapcsolatos további információkért lásd:

* A olvasásakor [az Event Hubs][lnk-getstarted-eh].
* A olvasásakor [Service Bus-üzenetsorok][lnk-getstarted-queue].
* A olvasásakor [Service Bus-üzenettémakörök][lnk-getstarted-topic].

### <a name="next-steps"></a>Következő lépések

IoT-központok végpontjai kapcsolatos további információkért lásd: [IoT-központok végpontjai][lnk-devguide-endpoints].

Hello lekérdezési nyelv kapcsolatos további információk toodefine útválasztási szabályokat használ, tekintse meg [IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási][lnk-devguide-query-language].

Hello [folyamat IoT Hub eszközről a felhőbe üzenetek útvonalak] [ lnk-d2c-tutorial] oktatóanyag bemutatja, hogyan toouse útválasztási szabályokat és egyéni végpontokat.

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md
