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
# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a><span data-ttu-id="f52fe-103">Az Azure Service Bus használata a WebJobs SDK-val</span><span class="sxs-lookup"><span data-stu-id="f52fe-103">How to use Azure Service Bus with the WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="f52fe-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f52fe-104">Overview</span></span>
<span data-ttu-id="f52fe-105">Ez az útmutató C# mintakódok bemutatják, hogyan indítható el egy folyamatot, az Azure Service Bus-üzenet fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="f52fe-105">This guide provides C# code samples that show how to trigger a process when an Azure Service Bus message is received.</span></span> <span data-ttu-id="f52fe-106">A kód minták használata [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verzió 1.x.</span><span class="sxs-lookup"><span data-stu-id="f52fe-106">The code samples use [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="f52fe-107">Az útmutató azt feltételezi, hogy tudja [hogyan webjobs-feladat-projekt létrehozása a Visual Studio kapcsolati karakterláncok a tárfiókhoz adott pontra](websites-dotnet-webjobs-sdk-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f52fe-107">The guide assumes you know [how to create a WebJob project in Visual Studio with connection strings that point to your storage account](websites-dotnet-webjobs-sdk-get-started.md).</span></span>

<span data-ttu-id="f52fe-108">A kódrészleteket csak megjelenítése funkciók, nem a kódot, amely létrehozza a `JobHost` objektum ebben a példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="f52fe-108">The code snippets only show functions, not the code that creates the `JobHost` object as in this example:</span></span>

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

<span data-ttu-id="f52fe-109">A [teljes Service Bus-példakód](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) az azure-webjobs-sdk-minták tárházban github.com van.</span><span class="sxs-lookup"><span data-stu-id="f52fe-109">A [complete Service Bus code example](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) is in the azure-webjobs-sdk-samples repository on GitHub.com.</span></span>

## <span data-ttu-id="f52fe-110"><a id="prerequisites"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f52fe-110"><a id="prerequisites"></a> Prerequisites</span></span>
<span data-ttu-id="f52fe-111">Telepítenie kell a Service Bus dolgozni a [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet-csomag mellett a többi WebJobs SDK-csomagot.</span><span class="sxs-lookup"><span data-stu-id="f52fe-111">To work with Service Bus you have to install the [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet package in addition to the other WebJobs SDK packages.</span></span> 

<span data-ttu-id="f52fe-112">Akkor is, a tárolási kapcsolati karakterláncok mellett AzureWebJobsServiceBus kapcsolati karakterlánc beállítása.</span><span class="sxs-lookup"><span data-stu-id="f52fe-112">You also have to set the AzureWebJobsServiceBus connection string in addition to the storage connection strings.</span></span>  <span data-ttu-id="f52fe-113">Ehhez a `connectionStrings` szakaszt az App.config fájl, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="f52fe-113">You can do this in the `connectionStrings` section of the App.config file, as shown in the following example:</span></span>

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

<span data-ttu-id="f52fe-114">Egy minta-projektet, amely tartalmazza a Service Bus kapcsolati karakterlánc beállítása az App.config fájlban, lásd: [példa Service Bus](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus).</span><span class="sxs-lookup"><span data-stu-id="f52fe-114">For a sample project that includes the Service Bus connection string setting in the App.config file, see [Service Bus example](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus).</span></span> 

<span data-ttu-id="f52fe-115">A kapcsolati karakterlánc is megadható az Azure futtatókörnyezetben, amely majd felülírja az App.config beállításokat, az Azure rendszerben; a webjobs-feladat futtatásakor További információkért lásd: [Ismerkedés a WebJobs SDK-val](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).</span><span class="sxs-lookup"><span data-stu-id="f52fe-115">The connection strings can also be set in the Azure runtime environment, which then overrides the App.config settings when the WebJob runs in Azure; for more information, see [Get Started with the WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).</span></span>

## <span data-ttu-id="f52fe-116"><a id="trigger"></a>Események indítása a következő függvényt egy Service Bus-üzenetsor fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="f52fe-116"><a id="trigger"></a> How to trigger a function when a Service Bus queue message is received</span></span>
<span data-ttu-id="f52fe-117">Egy függvényt, amely a WebJobs SDK meghívja a várólista-üzenet fogadásakor használja a `ServiceBusTrigger` attribútum.</span><span class="sxs-lookup"><span data-stu-id="f52fe-117">To write a function that the WebJobs SDK calls when a queue message is received, use the `ServiceBusTrigger` attribute.</span></span> <span data-ttu-id="f52fe-118">Az attribútum konstruktora paramétert, és kérdezze le a várólista nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="f52fe-118">The attribute constructor takes a parameter that specifies the name of the queue to poll.</span></span>

### <a name="how-servicebustrigger-works"></a><span data-ttu-id="f52fe-119">ServiceBusTrigger működése</span><span class="sxs-lookup"><span data-stu-id="f52fe-119">How ServiceBusTrigger works</span></span>
<span data-ttu-id="f52fe-120">Az SDK üzenetet kap `PeekLock` mód és a hívások `Complete` az üzenetet, ha a függvény futtatása sikeresen befejeződött, vagy a hívások `Abandon` Ha a parancs nem működik.</span><span class="sxs-lookup"><span data-stu-id="f52fe-120">The SDK receives a message in `PeekLock` mode and calls `Complete` on the message if the function finishes successfully, or calls `Abandon` if the function fails.</span></span> <span data-ttu-id="f52fe-121">Ha a függvény futásakor hosszabb, mint a `PeekLock` automatikusan megújítják időtúllépés, a zárolás.</span><span class="sxs-lookup"><span data-stu-id="f52fe-121">If the function runs longer than the `PeekLock` timeout, the lock is automatically renewed.</span></span>

<span data-ttu-id="f52fe-122">A Service Bus saját poison várólista kezelését, amely nem lehet ellenőrizni, és nem állította be a WebJobs SDK végzi.</span><span class="sxs-lookup"><span data-stu-id="f52fe-122">Service Bus does its own poison queue handling which cannot be controlled or configured by the WebJobs SDK.</span></span> 

### <a name="string-queue-message"></a><span data-ttu-id="f52fe-123">Karakterlánc üzenetsor</span><span class="sxs-lookup"><span data-stu-id="f52fe-123">String queue message</span></span>
<span data-ttu-id="f52fe-124">Az alábbi példakód egy üzenetsor-üzenetet, amely tartalmazza a karakterláncot, és írja a karakterlánc a WebJobs SDK irányítópult olvassa be.</span><span class="sxs-lookup"><span data-stu-id="f52fe-124">The following code sample reads a queue message that contains a string and writes the string to the WebJobs SDK dashboard.</span></span>

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

<span data-ttu-id="f52fe-125">**Megjegyzés:** egy alkalmazás, amely nem használja a WebJobs SDK létrehozásakor az üzenetsor-üzeneteket, feltétlenül állítson be [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) a "text/plain".</span><span class="sxs-lookup"><span data-stu-id="f52fe-125">**Note:** If you are creating the queue messages in an application that doesn't use the WebJobs SDK, make sure to set [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) to "text/plain".</span></span>

### <a name="poco-queue-message"></a><span data-ttu-id="f52fe-126">POCO üzenetsor</span><span class="sxs-lookup"><span data-stu-id="f52fe-126">POCO queue message</span></span>
<span data-ttu-id="f52fe-127">Az SDK automatikusan deszerializálni a egy üzenetsor-üzenetet, amely tartalmazza egy POCO JSON [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) típus.</span><span class="sxs-lookup"><span data-stu-id="f52fe-127">The SDK will automatically deserialize a queue message that contains JSON for a POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) type.</span></span> <span data-ttu-id="f52fe-128">A következő példakód olvas egy üzenetsor-üzenetet, amely tartalmaz egy `BlobInformation` objektum, amely rendelkezik egy `BlobName` tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="f52fe-128">The following code sample reads a queue message that contains a `BlobInformation` object which has a `BlobName` property:</span></span>

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

<span data-ttu-id="f52fe-129">A Kódminták bemutatja, hogyan tulajdonságainak a POCO használhatja a blobok és táblák ugyanabban a függvényben, tekintse meg a [tárolási várólisták verziója, ez a cikk](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).</span><span class="sxs-lookup"><span data-stu-id="f52fe-129">For code samples showing how to use properties of the POCO to work with blobs and tables in the same function, see the [storage queues version of this article](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).</span></span>

<span data-ttu-id="f52fe-130">Ha a kódot, amely az üzenetsor-üzenetet hoz létre a WebJobs SDK nem használ, akkor a kódot az alábbi példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="f52fe-130">If your code that creates the queue message doesn't use the WebJobs SDK, use code similar to the following example:</span></span>

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a><span data-ttu-id="f52fe-131">Típusok ServiceBusTrigger együttműködik</span><span class="sxs-lookup"><span data-stu-id="f52fe-131">Types ServiceBusTrigger works with</span></span>
<span data-ttu-id="f52fe-132">Emellett `string` és POCO típusok, használhatja a `ServiceBusTrigger` attribútum egy bájttömböt vagy egy `BrokeredMessage` objektum.</span><span class="sxs-lookup"><span data-stu-id="f52fe-132">Besides `string` and POCO  types, you can use the `ServiceBusTrigger` attribute with a byte array or a `BrokeredMessage` object.</span></span>

## <span data-ttu-id="f52fe-133"><a id="create"></a>A Service Bus-üzenetsor-üzeneteket létrehozása</span><span class="sxs-lookup"><span data-stu-id="f52fe-133"><a id="create"></a> How to create Service Bus queue messages</span></span>
<span data-ttu-id="f52fe-134">Egy függvényt, amely létrehoz egy új várólista-üzenet használata írni a `ServiceBus` attribútumot, és adja át a várólista neve, hogy az attribútum konstruktora.</span><span class="sxs-lookup"><span data-stu-id="f52fe-134">To write a function that creates a new queue message use the `ServiceBus` attribute and pass in the queue name to the attribute constructor.</span></span> 

### <a name="create-a-single-queue-message-in-a-non-async-function"></a><span data-ttu-id="f52fe-135">Egyetlen üzenetsor nem aszinkron függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="f52fe-135">Create a single queue message in a non-async function</span></span>
<span data-ttu-id="f52fe-136">Az alábbi példakód egy kimeneti paraméter használatával hozzon létre egy új üzenetet a várólistában, ugyanahhoz a tartalomhoz, mint az üzenet érkezett a várólista "inputqueue" nevű, "outputqueue" nevű.</span><span class="sxs-lookup"><span data-stu-id="f52fe-136">The following code sample uses an output parameter to create a new message in the queue named "outputqueue" with the same content as the message received in the queue named "inputqueue".</span></span>

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

<span data-ttu-id="f52fe-137">A kimeneti paraméter egyetlen üzenetsor létrehozásához a következő típusok egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="f52fe-137">The output parameter for creating a single queue message can be any of the following types:</span></span>

* `string`
* `byte[]`
* `BrokeredMessage`
* <span data-ttu-id="f52fe-138">A szerializálható POCO típus, amely adhat meg.</span><span class="sxs-lookup"><span data-stu-id="f52fe-138">A serializable POCO type that you define.</span></span> <span data-ttu-id="f52fe-139">Automatikusan szerializálható JSON-ként.</span><span class="sxs-lookup"><span data-stu-id="f52fe-139">Automatically serialized as JSON.</span></span>

<span data-ttu-id="f52fe-140">POCO típusú paraméterrel egy üzenetsor mindig létrejön, amikor befejeződik a függvény; a paraméterek egyike null értékű, ha az SDK-t egy NULL értékű lesz vissza, ha az üzenet fogadása és deszerializálni üzenetsor-üzenetet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f52fe-140">For POCO type parameters, a queue message is always created when the function ends; if the parameter is null, the SDK creates a queue message that will return null when the message is received and deserialized.</span></span> <span data-ttu-id="f52fe-141">A más típusú Ha a paraméter null értékű nincs üzenetsor jön létre.</span><span class="sxs-lookup"><span data-stu-id="f52fe-141">For the other types, if the parameter is null no queue message is created.</span></span>

### <a name="create-multiple-queue-messages-or-in-async-functions"></a><span data-ttu-id="f52fe-142">Hozzon létre több üzenetek vagy aszinkron függvény</span><span class="sxs-lookup"><span data-stu-id="f52fe-142">Create multiple queue messages or in async functions</span></span>
<span data-ttu-id="f52fe-143">Több üzenetet létrehozásához használja a `ServiceBus` rendelkező attribútum `ICollector<T>` vagy `IAsyncCollector<T>`, ahogy az az alábbi példakód:</span><span class="sxs-lookup"><span data-stu-id="f52fe-143">To create multiple messages, use  the `ServiceBus` attribute with `ICollector<T>` or `IAsyncCollector<T>`, as shown in the following code sample:</span></span>

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="f52fe-144">Minden várólista üzenet jön létre azonnal amikor a `Add` metódust.</span><span class="sxs-lookup"><span data-stu-id="f52fe-144">Each queue message is created immediately when the `Add` method is called.</span></span>

## <span data-ttu-id="f52fe-145"><a id="topics"></a>Service Bus-üzenettémakörök használata</span><span class="sxs-lookup"><span data-stu-id="f52fe-145"><a id="topics"></a>How to work with Service Bus topics</span></span>
<span data-ttu-id="f52fe-146">Az SDK-t meghívó, ha egy üzenet érkezik egy Service Bus-témakörbe függvényt használja a `ServiceBusTrigger` attribútumot a konstruktort témakör és előfizetés nevét, a következő kód mintában látható módon:</span><span class="sxs-lookup"><span data-stu-id="f52fe-146">To write a function that the SDK calls when a message is received on a Service Bus topic, use the `ServiceBusTrigger` attribute with the constructor that takes topic name and subscription name, as shown in the following code sample:</span></span>

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

<span data-ttu-id="f52fe-147">Hozzon létre egy üzenetet a témakör a, használja a `ServiceBus` attribútum használata a várólistacímke ugyanúgy témakör néven.</span><span class="sxs-lookup"><span data-stu-id="f52fe-147">To create a message on a topic, use the `ServiceBus` attribute with a topic name the same way you use it with a queue name.</span></span>

## <a name="features-added-in-release-11"></a><span data-ttu-id="f52fe-148">1.1-es verzióban hozzáadott funkciók</span><span class="sxs-lookup"><span data-stu-id="f52fe-148">Features added in release 1.1</span></span>
<span data-ttu-id="f52fe-149">A következő funkciók 1.1-es verzióban lettek hozzáadva:</span><span class="sxs-lookup"><span data-stu-id="f52fe-149">The following features were added in release 1.1:</span></span>

* <span data-ttu-id="f52fe-150">A részletes üzenetek feldolgozási keresztül testreszabásának engedélyezése `ServiceBusConfiguration.MessagingProvider`.</span><span class="sxs-lookup"><span data-stu-id="f52fe-150">Allow deep customization of message processing via `ServiceBusConfiguration.MessagingProvider`.</span></span>
* <span data-ttu-id="f52fe-151">`MessagingProvider`a Service Bus testreszabása támogatja `MessagingFactory` és `NamespaceManager`.</span><span class="sxs-lookup"><span data-stu-id="f52fe-151">`MessagingProvider` supports customization of the Service Bus `MessagingFactory` and `NamespaceManager`.</span></span>
* <span data-ttu-id="f52fe-152">A `MessageProcessor` stratégia mintát lehetővé teszi egy várólista/témakör processzort.</span><span class="sxs-lookup"><span data-stu-id="f52fe-152">A `MessageProcessor` strategy pattern allows you to specify a processor per queue/topic.</span></span>
* <span data-ttu-id="f52fe-153">Üzenet feldolgozása párhuzamossági alapértelmezés szerint támogatott.</span><span class="sxs-lookup"><span data-stu-id="f52fe-153">Message processing concurrency is supported by default.</span></span> 
* <span data-ttu-id="f52fe-154">Egyszerű testreszabás `OnMessageOptions` keresztül `ServiceBusConfiguration.MessageOptions`.</span><span class="sxs-lookup"><span data-stu-id="f52fe-154">Easy customization of `OnMessageOptions` via `ServiceBusConfiguration.MessageOptions`.</span></span>
* <span data-ttu-id="f52fe-155">Engedélyezése [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) meg kell adni a `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (a forgatókönyvekhez, amelyekben előfordulhat, hogy nem jogosultságaik kezelését).</span><span class="sxs-lookup"><span data-stu-id="f52fe-155">Allow [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) to be specified on `ServiceBusTriggerAttribute`/`ServiceBusAttribute` (for scenarios where you might not have Manage rights).</span></span> <span data-ttu-id="f52fe-156">Vegye figyelembe, hogy Azure webjobs-feladatok nem sikerült automatikusan kiépíteni a nem létező üzenetsorok és témakörök AccessRights kezelése nélkül.</span><span class="sxs-lookup"><span data-stu-id="f52fe-156">Note that Azure WebJobs is unable to automatically provision non-existent queues and topics without Manage AccessRights.</span></span>

## <span data-ttu-id="f52fe-157"><a id="queues"></a>A tároló – útmutató a várólisták cikkben említett kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="f52fe-157"><a id="queues"></a>Related topics covered by the storage queues how-to article</span></span>
<span data-ttu-id="f52fe-158">Nem adott Service Bus WebJobs SDK-forgatókönyvekkel kapcsolatos további információkért lásd: [Azure a queue storage használata a WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="f52fe-158">For information about WebJobs SDK scenarios not specific to Service Bus, see [How to use Azure queue storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="f52fe-159">A cikkben szereplő témakörök a következők:</span><span class="sxs-lookup"><span data-stu-id="f52fe-159">Topics covered in that article include the following:</span></span>

* <span data-ttu-id="f52fe-160">Aszinkron funkciók</span><span class="sxs-lookup"><span data-stu-id="f52fe-160">Async functions</span></span>
* <span data-ttu-id="f52fe-161">Több példánya</span><span class="sxs-lookup"><span data-stu-id="f52fe-161">Multiple instances</span></span>
* <span data-ttu-id="f52fe-162">Biztonságos leállításának</span><span class="sxs-lookup"><span data-stu-id="f52fe-162">Graceful shutdown</span></span>
* <span data-ttu-id="f52fe-163">Egy függvény törzséhez a WebJobs SDK attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="f52fe-163">Use WebJobs SDK attributes in the body of a function</span></span>
* <span data-ttu-id="f52fe-164">Az SDK-kapcsolati karakterláncok beállítása a kódban</span><span class="sxs-lookup"><span data-stu-id="f52fe-164">Set the SDK connection strings in code</span></span>
* <span data-ttu-id="f52fe-165">Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot</span><span class="sxs-lookup"><span data-stu-id="f52fe-165">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="f52fe-166">Manuálisan kezdeményezi egy függvény</span><span class="sxs-lookup"><span data-stu-id="f52fe-166">Trigger a function manually</span></span>
* <span data-ttu-id="f52fe-167">Naplók írása</span><span class="sxs-lookup"><span data-stu-id="f52fe-167">Write logs</span></span>

## <span data-ttu-id="f52fe-168"><a id="nextsteps"></a> Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f52fe-168"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="f52fe-169">Ez az útmutató nyújtott mintakódok, amelyek bemutatják, hogyan kezeli az Azure Service Bus használata gyakori forgatókönyvei.</span><span class="sxs-lookup"><span data-stu-id="f52fe-169">This guide has provided code samples that show how to handle common scenarios for working with Azure Service Bus.</span></span> <span data-ttu-id="f52fe-170">Azure webjobs-feladatok és a WebJobs SDK használatával kapcsolatos további információkért lásd: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="f52fe-170">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

