---
title: "Az Azure Service Bus használata a WebJobs SDK-val"
description: "Ismerje meg, hogyan használható az Azure Service Bus-üzenetsorok és témakörök a WebJobs SDK-val."
services: app-service\web, service-bus
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 2114a934-135b-42b8-871c-6cc040214e76
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 7cec03cae5d20d1ead9eb24e99415c33d8b76f05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>Az Azure Service Bus használata a WebJobs SDK-val
## <a name="overview"></a>Áttekintés
Ez az útmutató C# mintakódok bemutatják, hogyan indítható el egy folyamatot, az Azure Service Bus-üzenet fogadásakor. A kód minták használata [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verzió 1.x.

Az útmutató azt feltételezi, hogy tudja [hogyan webjobs-feladat-projekt létrehozása a Visual Studio kapcsolati karakterláncok a tárfiókhoz adott pontra](websites-dotnet-webjobs-sdk-get-started.md).

A kódrészleteket csak megjelenítése funkciók, nem a kódot, amely létrehozza a `JobHost` objektum ebben a példában látható módon:

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

A [teljes Service Bus-példakód](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) az azure-webjobs-sdk-minták tárházban github.com van.

## <a id="prerequisites"></a>Előfeltételek
Telepítenie kell a Service Bus dolgozni a [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet-csomag mellett a többi WebJobs SDK-csomagot. 

Akkor is, a tárolási kapcsolati karakterláncok mellett AzureWebJobsServiceBus kapcsolati karakterlánc beállítása.  Ehhez a `connectionStrings` szakaszt az App.config fájl, a következő példában látható módon:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Egy minta-projektet, amely tartalmazza a Service Bus kapcsolati karakterlánc beállítása az App.config fájlban, lásd: [példa Service Bus](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

A kapcsolati karakterlánc is megadható az Azure futtatókörnyezetben, amely majd felülírja az App.config beállításokat, az Azure rendszerben; a webjobs-feladat futtatásakor További információkért lásd: [Ismerkedés a WebJobs SDK-val](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a>Események indítása a következő függvényt egy Service Bus-üzenetsor fogadásakor.
Egy függvényt, amely a WebJobs SDK meghívja a várólista-üzenet fogadásakor használja a `ServiceBusTrigger` attribútum. Az attribútum konstruktora paramétert, és kérdezze le a várólista nevét adja meg.

### <a name="how-servicebustrigger-works"></a>ServiceBusTrigger működése
Az SDK üzenetet kap `PeekLock` mód és a hívások `Complete` az üzenetet, ha a függvény futtatása sikeresen befejeződött, vagy a hívások `Abandon` Ha a parancs nem működik. Ha a függvény futásakor hosszabb, mint a `PeekLock` automatikusan megújítják időtúllépés, a zárolás.

A Service Bus saját poison várólista kezelését, amely nem lehet ellenőrizni, és nem állította be a WebJobs SDK végzi. 

### <a name="string-queue-message"></a>Karakterlánc üzenetsor
Az alábbi példakód egy üzenetsor-üzenetet, amely tartalmazza a karakterláncot, és írja a karakterlánc a WebJobs SDK irányítópult olvassa be.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Megjegyzés:** egy alkalmazás, amely nem használja a WebJobs SDK létrehozásakor az üzenetsor-üzeneteket, feltétlenül állítson be [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) a "text/plain".

### <a name="poco-queue-message"></a>POCO üzenetsor
Az SDK automatikusan deszerializálni a egy üzenetsor-üzenetet, amely tartalmazza egy POCO JSON [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) típus. A következő példakód olvas egy üzenetsor-üzenetet, amely tartalmaz egy `BlobInformation` objektum, amely rendelkezik egy `BlobName` tulajdonság:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

A Kódminták bemutatja, hogyan tulajdonságainak a POCO használhatja a blobok és táblák ugyanabban a függvényben, tekintse meg a [tárolási várólisták verziója, ez a cikk](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Ha a kódot, amely az üzenetsor-üzenetet hoz létre a WebJobs SDK nem használ, akkor a kódot az alábbi példához hasonló:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Típusok ServiceBusTrigger együttműködik
Emellett `string` és POCO típusok, használhatja a `ServiceBusTrigger` attribútum egy bájttömböt vagy egy `BrokeredMessage` objektum.

## <a id="create"></a>A Service Bus-üzenetsor-üzeneteket létrehozása
Egy függvényt, amely létrehoz egy új várólista-üzenet használata írni a `ServiceBus` attribútumot, és adja át a várólista neve, hogy az attribútum konstruktora. 

### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Egyetlen üzenetsor nem aszinkron függvény létrehozása
Az alábbi példakód egy kimeneti paraméter használatával hozzon létre egy új üzenetet a várólistában, ugyanahhoz a tartalomhoz, mint az üzenet érkezett a várólista "inputqueue" nevű, "outputqueue" nevű.

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

A kimeneti paraméter egyetlen üzenetsor létrehozásához a következő típusok egyike lehet:

* `string`
* `byte[]`
* `BrokeredMessage`
* A szerializálható POCO típus, amely adhat meg. Automatikusan szerializálható JSON-ként.

POCO típusú paraméterrel egy üzenetsor mindig létrejön, amikor befejeződik a függvény; a paraméterek egyike null értékű, ha az SDK-t egy NULL értékű lesz vissza, ha az üzenet fogadása és deszerializálni üzenetsor-üzenetet hoz létre. A más típusú Ha a paraméter null értékű nincs üzenetsor jön létre.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Hozzon létre több üzenetek vagy aszinkron függvény
Több üzenetet létrehozásához használja a `ServiceBus` rendelkező attribútum `ICollector<T>` vagy `IAsyncCollector<T>`, ahogy az az alábbi példakód:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Minden várólista üzenet jön létre azonnal amikor a `Add` metódust.

## <a id="topics"></a>Service Bus-üzenettémakörök használata
Az SDK-t meghívó, ha egy üzenet érkezik egy Service Bus-témakörbe függvényt használja a `ServiceBusTrigger` attribútumot a konstruktort témakör és előfizetés nevét, a következő kód mintában látható módon:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

Hozzon létre egy üzenetet a témakör a, használja a `ServiceBus` attribútum használata a várólistacímke ugyanúgy témakör néven.

## <a name="features-added-in-release-11"></a>1.1-es verzióban hozzáadott funkciók
A következő funkciók 1.1-es verzióban lettek hozzáadva:

* A részletes üzenetek feldolgozási keresztül testreszabásának engedélyezése `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`a Service Bus testreszabása támogatja `MessagingFactory` és `NamespaceManager`.
* A `MessageProcessor` stratégia mintát lehetővé teszi egy várólista/témakör processzort.
* Üzenet feldolgozása párhuzamossági alapértelmezés szerint támogatott. 
* Egyszerű testreszabás `OnMessageOptions` keresztül `ServiceBusConfiguration.MessageOptions`.
* Engedélyezése [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) meg kell adni a `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (a forgatókönyvekhez, amelyekben előfordulhat, hogy nem jogosultságaik kezelését). Vegye figyelembe, hogy Azure webjobs-feladatok nem sikerült automatikusan kiépíteni a nem létező üzenetsorok és témakörök AccessRights kezelése nélkül.

## <a id="queues"></a>A tároló – útmutató a várólisták cikkben említett kapcsolódó témakörök
Nem adott Service Bus WebJobs SDK-forgatókönyvekkel kapcsolatos további információkért lásd: [Azure a queue storage használata a WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

A cikkben szereplő témakörök a következők:

* Aszinkron funkciók
* Több példánya
* Biztonságos leállításának
* Egy függvény törzséhez a WebJobs SDK attribútumok használata
* Az SDK-kapcsolati karakterláncok beállítása a kódban
* Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot
* Manuálisan kezdeményezi egy függvény
* Naplók írása

## <a id="nextsteps"></a> Következő lépések
Ez az útmutató nyújtott mintakódok, amelyek bemutatják, hogyan kezeli az Azure Service Bus használata gyakori forgatókönyvei. Azure webjobs-feladatok és a WebJobs SDK használatával kapcsolatos további információkért lásd: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).

