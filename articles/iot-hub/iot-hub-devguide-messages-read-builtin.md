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
# <a name="read-device-to-cloud-messages-from-hello-built-in-endpoint"></a><span data-ttu-id="cda5a-103">Eszköz a felhőbe küldött üzeneteket beolvasni hello beépített végpont</span><span class="sxs-lookup"><span data-stu-id="cda5a-103">Read device-to-cloud messages from hello built-in endpoint</span></span>

<span data-ttu-id="cda5a-104">Üzenetek alapesetben irányított toohello beépített szolgáltatás felé néző végpont (**üzenetek/események**), amely kompatibilis a [Event Hubs][lnk-event-hubs].</span><span class="sxs-lookup"><span data-stu-id="cda5a-104">By default, messages are routed toohello built-in service-facing endpoint (**messages/events**), that is compatible with [Event Hubs][lnk-event-hubs].</span></span> <span data-ttu-id="cda5a-105">Ehhez a végponthoz jelenleg csak akkor jelennek meg hello segítségével [AMQP] [ lnk-amqp] protokoll az 5671-es port.</span><span class="sxs-lookup"><span data-stu-id="cda5a-105">This endpoint is currently only exposed using hello [AMQP][lnk-amqp] protocol on port 5671.</span></span> <span data-ttu-id="cda5a-106">Az IoT-központ közzététele hello következő tulajdonságok tooenable meg toocontrol hello beépített Event Hub-kompatibilis üzenetkezelési végpont **üzenetek/események**.</span><span class="sxs-lookup"><span data-stu-id="cda5a-106">An IoT hub exposes hello following properties tooenable you toocontrol hello built-in Event Hub-compatible messaging endpoint **messages/events**.</span></span>

| <span data-ttu-id="cda5a-107">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cda5a-107">Property</span></span>            | <span data-ttu-id="cda5a-108">Leírás</span><span class="sxs-lookup"><span data-stu-id="cda5a-108">Description</span></span> |
| ------------------- | ----------- |
| <span data-ttu-id="cda5a-109">**Partíciók száma**</span><span class="sxs-lookup"><span data-stu-id="cda5a-109">**Partition count**</span></span> | <span data-ttu-id="cda5a-110">Állítsa be megfelelően a létrehozási toodefine hello száma [partíciók] [ lnk-event-hub-partitions] eszközről a felhőbe esemény szempontjából.</span><span class="sxs-lookup"><span data-stu-id="cda5a-110">Set this property at creation toodefine hello number of [partitions][lnk-event-hub-partitions] for device-to-cloud event ingestion.</span></span> |
| <span data-ttu-id="cda5a-111">**Megőrzési idő**</span><span class="sxs-lookup"><span data-stu-id="cda5a-111">**Retention time**</span></span>  | <span data-ttu-id="cda5a-112">Ez a tulajdonság meghatározza, hogy az üzenetek megmaradnak az IoT-központ által nap.</span><span class="sxs-lookup"><span data-stu-id="cda5a-112">This property specifies how long in days messages are retained by IoT Hub.</span></span> <span data-ttu-id="cda5a-113">hello alapértelmezés szerint egy nap, de nagyobb tooseven nap lehet.</span><span class="sxs-lookup"><span data-stu-id="cda5a-113">hello default is one day, but it can be increased tooseven days.</span></span> |

<span data-ttu-id="cda5a-114">Az IoT-központ emellett lehetővé teszi a fogyasztói csoportok toomanage hello beépített eszköz-felhő végpont fogadására.</span><span class="sxs-lookup"><span data-stu-id="cda5a-114">IoT Hub also enables you toomanage consumer groups on hello built-in device-to-cloud receive endpoint.</span></span>

<span data-ttu-id="cda5a-115">Alapértelmezés szerint minden üzenetet, amely explicit módon a szabálynak üzenet útválasztási írt toohello beépített végpont.</span><span class="sxs-lookup"><span data-stu-id="cda5a-115">By default, all messages that do not explicitly match a message routing rule are written toohello built-in endpoint.</span></span> <span data-ttu-id="cda5a-116">Ha letiltja ezt a tartalék útvonalat, eldobott üzenetek, amely explicit módon nem egyezik a minden üzenet útválasztási szabályokat.</span><span class="sxs-lookup"><span data-stu-id="cda5a-116">If you disable this fallback route, messages that do not explicitly match any message routing rules are dropped.</span></span>

<span data-ttu-id="cda5a-117">Módosíthatja a megőrzési idő hello, vagy programozott módon hello [IoT-központ erőforrás-szolgáltató REST API-k][lnk-resource-provider-apis], vagy hello segítségével [Azure-portálon] [ lnk-management-portal].</span><span class="sxs-lookup"><span data-stu-id="cda5a-117">You can modify hello retention time, either programmatically through hello [IoT Hub resource provider REST APIs][lnk-resource-provider-apis], or by using hello [Azure portal][lnk-management-portal].</span></span>

<span data-ttu-id="cda5a-118">Az IoT-központ közzététele hello **üzenetek/események** beépített végpont a háttér-szolgáltatások tooread eszközről a felhőbe köszönőüzenetei a központ által fogadott.</span><span class="sxs-lookup"><span data-stu-id="cda5a-118">IoT Hub exposes hello **messages/events** built-in endpoint for your back-end services tooread hello device-to-cloud messages received by your hub.</span></span> <span data-ttu-id="cda5a-119">Ezt a végpontot az esemény lehetővé teszi bármely hello mechanizmusok hello Event Hubs szolgáltatás támogatja az üzenetek olvasásához toouse Hub-kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="cda5a-119">This endpoint is Event Hub-compatible, which enables you toouse any of hello mechanisms hello Event Hubs service supports for reading messages.</span></span>

## <a name="read-from-hello-built-in-endpoint"></a><span data-ttu-id="cda5a-120">Olvassa el a hello beépített végpont</span><span class="sxs-lookup"><span data-stu-id="cda5a-120">Read from hello built-in endpoint</span></span>

<span data-ttu-id="cda5a-121">Hello használata esetén [Azure Service Bus SDK for .NET] [ lnk-servicebus-sdk] vagy hello [Event Hubs - Event Processor Host][lnk-eventprocessorhost], minden IoT Hub-kapcsolat karakterláncok hello megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="cda5a-121">When you use hello [Azure Service Bus SDK for .NET][lnk-servicebus-sdk] or hello [Event Hubs - Event Processor Host][lnk-eventprocessorhost], you can use any IoT Hub connection strings with hello correct permissions.</span></span> <span data-ttu-id="cda5a-122">Ezután **üzenetek/események** hello Eseményközpont neveként.</span><span class="sxs-lookup"><span data-stu-id="cda5a-122">Then use **messages/events** as hello Event Hub name.</span></span>

<span data-ttu-id="cda5a-123">Amikor az SDK-k (vagy a termék integrációja), amely nem tudnak a IoT Hub, az Event Hub-kompatibilis végpont és Event Hub-kompatibilis nevét kell lekérnie az hello hello IoT-központ beállításai [Azure-portálon] [ lnk-management-portal]:</span><span class="sxs-lookup"><span data-stu-id="cda5a-123">When you use SDKs (or product integrations) that are unaware of IoT Hub, you must retrieve an Event Hub-compatible endpoint and Event Hub-compatible name from hello IoT Hub settings in hello [Azure portal][lnk-management-portal]:</span></span>

1. <span data-ttu-id="cda5a-124">Hello IoT hub paneljén kattintson **végpontok**.</span><span class="sxs-lookup"><span data-stu-id="cda5a-124">In hello IoT hub blade, click **Endpoints**.</span></span>
1. <span data-ttu-id="cda5a-125">A hello **beépített végpontok** kattintson **események**.</span><span class="sxs-lookup"><span data-stu-id="cda5a-125">In hello **Built-in endpoints** section, click **Events**.</span></span> <span data-ttu-id="cda5a-126">hello panel tartalmazza a következő értékek hello: **Event Hub-kompatibilis végpont**, **Event Hub-kompatibilis neve**, **partíciók**, **megőrzésiidőtartama**, és **fogyasztói csoportok**.</span><span class="sxs-lookup"><span data-stu-id="cda5a-126">hello blade contains hello following values: **Event Hub-compatible endpoint**, **Event Hub-compatible name**, **Partitions**, **Retention time**, and **Consumer groups**.</span></span>

    ![Eszköz-felhő beállításai][img-eventhubcompatible]

<span data-ttu-id="cda5a-128">az IoT Hub SDK hello hello IoT-központ végpont neve, amely szükséges **üzenetek/események** látható módon hello **végpontok** panelen.</span><span class="sxs-lookup"><span data-stu-id="cda5a-128">hello IoT Hub SDK requires hello IoT Hub endpoint name, which is **messages/events** as shown in hello **Endpoints** blade.</span></span>

<span data-ttu-id="cda5a-129">Ha hello SDK használata szükséges egy **állomásnév** vagy **Namespace** érték, hello séma eltávolítása hello **Event Hub-kompatibilis végpont**.</span><span class="sxs-lookup"><span data-stu-id="cda5a-129">If hello SDK you are using requires a **Hostname** or **Namespace** value, remove hello scheme from hello **Event Hub-compatible endpoint**.</span></span> <span data-ttu-id="cda5a-130">Például, ha az Event Hub-kompatibilis végpont **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, hello **állomásnév** lenne  **IOT hubbal-ns-myiothub-1234.servicebus.windows.net**, és hello **Namespace** lenne **IOT hubbal-ns-myiothub-1234**.</span><span class="sxs-lookup"><span data-stu-id="cda5a-130">For example, if your Event Hub-compatible endpoint is **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, hello **Hostname** would be **iothub-ns-myiothub-1234.servicebus.windows.net**, and hello **Namespace** would be **iothub-ns-myiothub-1234**.</span></span>

<span data-ttu-id="cda5a-131">Minden megosztott elérési házirendet, amely rendelkezik hello használhatók **ServiceConnect** engedélyek tooconnect toohello megadott Eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="cda5a-131">You can then use any shared access policy that has hello **ServiceConnect** permissions tooconnect toohello specified Event Hub.</span></span>

<span data-ttu-id="cda5a-132">Ha hello korábbi információk segítségével kell toobuild egy Eseményközpontba kapcsolati karakterláncot, használja a hello mintát a következő:</span><span class="sxs-lookup"><span data-stu-id="cda5a-132">If you need toobuild an Event Hub connection string by using hello previous information, use hello following pattern:</span></span>

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

<span data-ttu-id="cda5a-133">hello SDK-k és az IoT-központ elérhetővé tévő Event Hub-kompatibilis végpontokon használható integrációja a következő lista hello hello elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="cda5a-133">hello SDKs and integrations that you can use with Event Hub-compatible endpoints that IoT Hub exposes includes hello items in hello following list:</span></span>

* <span data-ttu-id="cda5a-134">[Az Event Hubs Java ügyfél](https://github.com/hdinsight/eventhubs-client).</span><span class="sxs-lookup"><span data-stu-id="cda5a-134">[Java Event Hubs client](https://github.com/hdinsight/eventhubs-client).</span></span>
* <span data-ttu-id="cda5a-135">[Apache Storm spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="cda5a-135">[Apache Storm spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md).</span></span> <span data-ttu-id="cda5a-136">Megtekintheti a hello [forrás spout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) a Githubon.</span><span class="sxs-lookup"><span data-stu-id="cda5a-136">You can view hello [spout source](https://github.com/apache/storm/tree/master/external/storm-eventhubs) on GitHub.</span></span>
* <span data-ttu-id="cda5a-137">[Apache Spark-integráció](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md).</span><span class="sxs-lookup"><span data-stu-id="cda5a-137">[Apache Spark integration](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cda5a-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cda5a-138">Next steps</span></span>

<span data-ttu-id="cda5a-139">IoT-központok végpontjai kapcsolatos további információkért lásd: [IoT-központok végpontjai][lnk-endpoints].</span><span class="sxs-lookup"><span data-stu-id="cda5a-139">For more information about IoT Hub endpoints, see [IoT Hub endpoints][lnk-endpoints].</span></span>

<span data-ttu-id="cda5a-140">Hello [Ismerkedés] [ lnk-get-started] oktatóanyagok megjeleníteni hogyan toosend eszközről a felhőbe érkező üzenetek eszközök szimulált és hello beépített végpont köszönőüzenetei olvasni.</span><span class="sxs-lookup"><span data-stu-id="cda5a-140">hello [Get Started][lnk-get-started] tutorials show you how toosend device-to-cloud messages from simulated devices and read hello messages from hello built-in endpoint.</span></span> <span data-ttu-id="cda5a-141">További részletekért lásd: hello [folyamat IoT Hub eszközről a felhőbe üzenetek útvonalak] [ lnk-d2c-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="cda5a-141">For more detail, see hello [Process IoT Hub device-to-cloud messages using routes][lnk-d2c-tutorial] tutorial.</span></span>

<span data-ttu-id="cda5a-142">Ha azt szeretné, hogy tooroute az eszköz-felhő üzenetek toocustom végpontok című [üzenet útvonalak és egyéni végpontokat eszközről a felhőbe üzeneteiben használt][lnk-custom].</span><span class="sxs-lookup"><span data-stu-id="cda5a-142">If you want tooroute your device-to-cloud messages toocustom endpoints, see [Use message routes and custom endpoints for device-to-cloud messages][lnk-custom].</span></span>

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
