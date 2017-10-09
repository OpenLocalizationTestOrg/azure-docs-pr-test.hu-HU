---
title: aaaHow toouse Azure Service Bus hello WebJobs SDK a
description: "Ismerje meg, hogyan toouse Azure Service Bus-üzenetsorok és témakörök a hello WebJobs SDK."
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
ms.openlocfilehash: cb801a9320a20c276da4f48c8941c09d3f09bb1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-service-bus-with-hello-webjobs-sdk"></a>Hogyan toouse Azure Service Bus a hello WebJobs SDK
## <a name="overview"></a>Áttekintés
Ez az útmutató C# kódot, hogy hogyan minták tootrigger egy folyamatot, az Azure Service Bus-üzenet fogadásakor. hello kód minták használata [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verzió 1.x.

hello az útmutató feltételezi, hogy tudja [hogyan toocreate egy webjobs-feladat projektet, a Visual Studio kapcsolati karakterláncok adott pont tooyour tárfiók](websites-dotnet-webjobs-sdk-get-started.md).

hello kódtöredékek csak megjelenítése funkciók nem hello hello létrehozó kód mellől `JobHost` objektum ebben a példában látható módon:

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

A [teljes Service Bus-példakód](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) hello azure-webjobs-sdk-minták tárházban github.com van.

## <a id="prerequisites"></a>Előfeltételek
toowork Service Bus az informatikai részleg tooinstall hello [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet csomag továbbá toohello többi WebJobs SDK-csomagot. 

Továbbá toohello tárolási kapcsolati karakterláncok tooset hello AzureWebJobsServiceBus kapcsolati karakterláncot is.  Ezt megteheti hello `connectionStrings` hello App.config fájl, ahogy az alábbi példa hello szakaszában:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Egy minta-projektet, amely tartalmazza az hello Service Bus kapcsolati karakterlánc beállítása hello App.config fájlban, lásd: [példa Service Bus](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

hello kapcsolati karakterláncok is megadható hello Azure futtatókörnyezetben, amely majd felülírja a hello App.config beállításokat, az Azure rendszerben; hello webjobs-feladat futtatásakor További információkért lásd: [Ismerkedés a WebJobs SDK hello](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a>Hogyan tootrigger egy függvényt, amikor egy Service Bus feldolgozási sor üzenetet érkezett
toowrite egy függvényt, amely a WebJobs SDK hello meghívja a várólista-üzenet fogadásakor, használja a hello `ServiceBusTrigger` attribútum. az attribútum konstruktora hello paramétert hello várólista toopoll hello nevét adja meg.

### <a name="how-servicebustrigger-works"></a>ServiceBusTrigger működése
hello SDK üzenetet kap a `PeekLock` mód és a hívások `Complete` üdvözlőüzenetére Ha hello függvény futtatása sikeresen befejeződött, vagy a hívások `Abandon` hello függvény meghibásodásakor. Ha hello függvény futásakor hello hosszabb `PeekLock` időkorlát, hello zárolása automatikusan megújítják.

A Service Bus saját nem ellenőrzött vagy hello WebJobs SDK által konfigurált poison várólista kezelését végzi. 

### <a name="string-queue-message"></a>Karakterlánc üzenetsor
hello alábbi kódminta olvassa be, amely tartalmazza a karakterláncot, majd beírja hello karakterlánc toohello WebJobs SDK irányítópult várólista üzenet.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Megjegyzés:** létrehozásakor hello üzenetsor-üzeneteket egy alkalmazás, amely nem használja a WebJobs SDK hello, győződjön meg arról, hogy tooset [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) túl "text/plain".

### <a name="poco-queue-message"></a>POCO üzenetsor
hello SDK automatikusan deszerializálni a egy üzenetsor-üzenetet, amely tartalmazza egy POCO JSON [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) típus. hello alábbi kódminta olvas egy üzenetsor-üzenetet, amely tartalmaz egy `BlobInformation` objektum, amely rendelkezik egy `BlobName` tulajdonság:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

Hogyan hello POCO toowork blobok és táblák toouse tulajdonságainak hello azonos megjelenítő mintakódok működik, tekintse meg a hello [tárolási várólisták verziója, ez a cikk](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Ha a várólista üdvözlőüzenetére létrehozó kód hello WebJobs SDK nem használ, akkor a kód hasonló toohello a következő példa:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Típusok ServiceBusTrigger együttműködik
Módosításokon kívül `string` és POCO típusok hello használhatja `ServiceBusTrigger` attribútum egy bájttömböt vagy egy `BrokeredMessage` objektum.

## <a id="create"></a>Hogyan toocreate Service Bus várólistára üzenetek
egy függvényt, amely létrehoz egy új sor üzenetet toowrite hello használata `ServiceBus` attribútum számára pedig hello várólista neve toohello attribútumok konstruktorában. 

### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Egyetlen üzenetsor nem aszinkron függvény létrehozása
a következő példakód hello egy új üzenet hello várólista neve "outputqueue" hello "inputqueue" nevű hello várólista fogadott üzenet hello ugyanaz, mint a tartalom a kimeneti paraméter toocreate használja.

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

egyetlen üzenetsor létrehozásához hello kimeneti paraméter hello a következő típusok egyike lehet:

* `string`
* `byte[]`
* `BrokeredMessage`
* A szerializálható POCO típus, amely adhat meg. Automatikusan szerializálható JSON-ként.

POCO típusú paraméterrel egy üzenetsor mindig létrejön, amikor befejeződik hello függvény; hello paraméterek egyike null értékű, ha hello SDK egy NULL értékű lesz vissza, ha az üdvözlőüzenetére fogad és deszerializálni üzenetsor-üzenetet hoz létre. Az egyéb hello, ha hello paraméterek egyike null értékű nincs várólista üzenet jön létre.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Hozzon létre több üzenetek vagy aszinkron függvény
toocreate több üzenetet hello használata `ServiceBus` rendelkező attribútum `ICollector<T>` vagy `IAsyncCollector<T>`, ahogy az alábbi példakód hello:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Minden várólista-üzenet létrehozásakor a rendszer azonnal hello `Add` metódust.

## <a id="topics"></a>Hogyan toowork a Service Bus-témakörök
toowrite hello SDK függvény meghívja a Service Bus-témakörbe üzenet fogadásakor, használja a hello `ServiceBusTrigger` hello konstruktort témakör és előfizetés nevét, ahogy az alábbi példakód hello attribútum:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

egy üzenettémakört, használjon hello üzenet toocreate `ServiceBus` attribútum egy témakör neve hello az azonos módon a várólistacímke együtt használja azt.

## <a name="features-added-in-release-11"></a>1.1-es verzióban hozzáadott funkciók
a következő funkciók hello 1.1-es verzióban lettek hozzáadva:

* A részletes üzenetek feldolgozási keresztül testreszabásának engedélyezése `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`támogatja a Service Bus hello testreszabása `MessagingFactory` és `NamespaceManager`.
* A `MessageProcessor` stratégia mintát lehetővé teszi a várólista/témakör / processzort toospecify.
* Üzenet feldolgozása párhuzamossági alapértelmezés szerint támogatott. 
* Egyszerű testreszabás `OnMessageOptions` keresztül `ServiceBusConfiguration.MessageOptions`.
* Engedélyezése [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) megadott toobe `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (a forgatókönyvekhez, amelyekben előfordulhat, hogy nem jogosultságaik kezelését). Ne feledje, hogy Azure webjobs-feladatok nem tooautomatically kiépítése nem létező üzenetsorok és témakörök AccessRights kezelése nélkül.

## <a id="queues"></a>Hello tárolási várólisták hogyan-tooarticle hatálya kapcsolódó témakörök
Nem adott tooService Bus, lásd: WebJobs SDK-forgatókönyvekkel kapcsolatos információk [hogyan toouse Azure várólista tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

A cikkben szereplő témakörök hello alábbiakat foglalja magába:

* Aszinkron funkciók
* Több példánya
* Biztonságos leállításának
* Egy függvény törzséhez hello a WebJobs SDK attribútumok használata
* A kód hello SDK kapcsolati karakterláncok beállítása
* Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot
* Manuálisan kezdeményezi egy függvény
* Naplók írása

## <a id="nextsteps"></a> Következő lépések
Ez az útmutató nyújtott a kódot, hogy hogyan minták toohandle gyakori forgatókönyvei Azure Service Bus használata. További információ a hogyan toouse Azure webjobs-feladatok és a WebJobs SDK hello: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).

