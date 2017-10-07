---
title: "aaaUnderstand hello Azure IoT Hub beépített végpont |} Microsoft Docs"
description: "Fejlesztői útmutató - ismerteti, hogyan toouse hello beépített, Event Hub-kompatibilis végpont toread eszköz-a-felhőbe küldött üzeneteket."
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
ms.openlocfilehash: 15484c1b1828151ffcae5f4a1407264374223da1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="read-device-to-cloud-messages-from-hello-built-in-endpoint"></a>Eszköz a felhőbe küldött üzeneteket beolvasni hello beépített végpont

Üzenetek alapesetben irányított toohello beépített szolgáltatás felé néző végpont (**üzenetek/események**), amely kompatibilis a [Event Hubs][lnk-event-hubs]. Ehhez a végponthoz jelenleg csak akkor jelennek meg hello segítségével [AMQP] [ lnk-amqp] protokoll az 5671-es port. Az IoT-központ közzététele hello következő tulajdonságok tooenable meg toocontrol hello beépített Event Hub-kompatibilis üzenetkezelési végpont **üzenetek/események**.

| Tulajdonság            | Leírás |
| ------------------- | ----------- |
| **Partíciók száma** | Állítsa be megfelelően a létrehozási toodefine hello száma [partíciók] [ lnk-event-hub-partitions] eszközről a felhőbe esemény szempontjából. |
| **Megőrzési idő**  | Ez a tulajdonság meghatározza, hogy az üzenetek megmaradnak az IoT-központ által nap. hello alapértelmezés szerint egy nap, de nagyobb tooseven nap lehet. |

Az IoT-központ emellett lehetővé teszi a fogyasztói csoportok toomanage hello beépített eszköz-felhő végpont fogadására.

Alapértelmezés szerint minden üzenetet, amely explicit módon a szabálynak üzenet útválasztási írt toohello beépített végpont. Ha letiltja ezt a tartalék útvonalat, eldobott üzenetek, amely explicit módon nem egyezik a minden üzenet útválasztási szabályokat.

Módosíthatja a megőrzési idő hello, vagy programozott módon hello [IoT-központ erőforrás-szolgáltató REST API-k][lnk-resource-provider-apis], vagy hello segítségével [Azure-portálon] [ lnk-management-portal].

Az IoT-központ közzététele hello **üzenetek/események** beépített végpont a háttér-szolgáltatások tooread eszközről a felhőbe köszönőüzenetei a központ által fogadott. Ezt a végpontot az esemény lehetővé teszi bármely hello mechanizmusok hello Event Hubs szolgáltatás támogatja az üzenetek olvasásához toouse Hub-kompatibilis.

## <a name="read-from-hello-built-in-endpoint"></a>Olvassa el a hello beépített végpont

Hello használata esetén [Azure Service Bus SDK for .NET] [ lnk-servicebus-sdk] vagy hello [Event Hubs - Event Processor Host][lnk-eventprocessorhost], minden IoT Hub-kapcsolat karakterláncok hello megfelelő engedélyekkel. Ezután **üzenetek/események** hello Eseményközpont neveként.

Amikor az SDK-k (vagy a termék integrációja), amely nem tudnak a IoT Hub, az Event Hub-kompatibilis végpont és Event Hub-kompatibilis nevét kell lekérnie az hello hello IoT-központ beállításai [Azure-portálon] [ lnk-management-portal]:

1. Hello IoT hub paneljén kattintson **végpontok**.
1. A hello **beépített végpontok** kattintson **események**. hello panel tartalmazza a következő értékek hello: **Event Hub-kompatibilis végpont**, **Event Hub-kompatibilis neve**, **partíciók**, **megőrzésiidőtartama**, és **fogyasztói csoportok**.

    ![Eszköz-felhő beállításai][img-eventhubcompatible]

az IoT Hub SDK hello hello IoT-központ végpont neve, amely szükséges **üzenetek/események** látható módon hello **végpontok** panelen.

Ha hello SDK használata szükséges egy **állomásnév** vagy **Namespace** érték, hello séma eltávolítása hello **Event Hub-kompatibilis végpont**. Például, ha az Event Hub-kompatibilis végpont **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, hello **állomásnév** lenne  **IOT hubbal-ns-myiothub-1234.servicebus.windows.net**, és hello **Namespace** lenne **IOT hubbal-ns-myiothub-1234**.

Minden megosztott elérési házirendet, amely rendelkezik hello használhatók **ServiceConnect** engedélyek tooconnect toohello megadott Eseményközpontot.

Ha hello korábbi információk segítségével kell toobuild egy Eseményközpontba kapcsolati karakterláncot, használja a hello mintát a következő:

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

hello SDK-k és az IoT-központ elérhetővé tévő Event Hub-kompatibilis végpontokon használható integrációja a következő lista hello hello elemeket tartalmazza:

* [Az Event Hubs Java ügyfél](https://github.com/hdinsight/eventhubs-client).
* [Apache Storm spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). Megtekintheti a hello [forrás spout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) a Githubon.
* [Apache Spark-integráció](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md).

## <a name="next-steps"></a>Következő lépések

IoT-központok végpontjai kapcsolatos további információkért lásd: [IoT-központok végpontjai][lnk-endpoints].

Hello [Ismerkedés] [ lnk-get-started] oktatóanyagok megjeleníteni hogyan toosend eszközről a felhőbe érkező üzenetek eszközök szimulált és hello beépített végpont köszönőüzenetei olvasni. További részletekért lásd: hello [folyamat IoT Hub eszközről a felhőbe üzenetek útvonalak] [ lnk-d2c-tutorial] oktatóanyag.

Ha azt szeretné, hogy tooroute az eszköz-felhő üzenetek toocustom végpontok című [üzenet útvonalak és egyéni végpontokat eszközről a felhőbe üzeneteiben használt][lnk-custom].

[img-eventhubcompatible]: ./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png

[lnk-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-management-portal]: https://portal.azure.com
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-event-hub-partitions]: ../event-hubs/event-hubs-features.md#partitions
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx
[lnk-amqp]: https://www.amqp.org/
